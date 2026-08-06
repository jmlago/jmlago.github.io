# A Stupidly Simple Multi-Agent Orchestrator with tmux

One Codex supervising other Codex and Claude Code processes, with no framework in between.

## The premise

Coding agents are already processes. They read input, do work, print output, occasionally get stuck, and eventually exit. So before reaching for an orchestration framework, it is worth asking a rude question:

> What if the multi-agent control plane is just tmux?

tmux gives us persistent processes, stable pane identifiers, a way to send input, a way to capture output, and a screen a human can attach to at any moment. It does not care whether a pane contains Codex, Claude Code, a debugger, or a shell script.

That is enough to build a surprisingly useful local orchestrator.

## The experiment

I tested this with tmux 3.6a, Codex CLI 0.146.0, and Claude Code 2.1.220. The supervisor created one session with two panes, launched one Codex worker and one Claude worker in parallel, collected both results, and handed them to another Codex for synthesis.

No MCP server. No message broker. No agent framework.

## Build the control plane

Create an isolated scratch directory and two worker panes:

~~~bash
lab_root=$(mktemp -d -t agent-lab.XXXXXX)

tmux new-session -d -s agent-lab -n workers -c "$lab_root"
tmux split-window -h -t agent-lab:0 -c "$lab_root"
tmux select-layout -t agent-lab:0 even-horizontal

codex_pane=$(tmux display-message -p -t agent-lab:0.0 '#{pane_id}')
claude_pane=$(tmux display-message -p -t agent-lab:0.1 '#{pane_id}')
~~~

The pane IDs matter. Window positions such as <code>agent-lab:0.0</code> can drift after a pane crashes or a human rearranges the session. IDs such as <code>%1</code> remain attached to the actual pane.

You can watch the whole system with:

~~~bash
tmux attach -t agent-lab
~~~

Detach with <code>Ctrl-b d</code>. The workers keep running.

## Launch heterogeneous workers

For orchestration, non-interactive invocations are easier to reason about than scraping full-screen TUIs. tmux still gives us persistence and observability, while result and status files give us an unambiguous completion protocol.

~~~bash
codex_command="codex exec --ephemeral --sandbox read-only \
  --skip-git-repo-check -C '$lab_root' \
  -o '$lab_root/codex.out' \
  'Design a four-state protocol for a tmux agent worker. Do not use tools.'; \
  printf '%s\n' \$? > '$lab_root/codex.status'"

claude_command="claude -p --effort low --tools '' \
  --permission-mode plan --no-session-persistence \
  --max-budget-usd 0.25 \
  'List four failure modes of a tmux agent orchestrator. Do not use tools.' \
  > '$lab_root/claude.out' 2>&1; \
  printf '%s\n' \$? > '$lab_root/claude.status'"

tmux send-keys -l -t "$codex_pane" "$codex_command"
tmux send-keys -t "$codex_pane" Enter

tmux send-keys -l -t "$claude_pane" "$claude_command"
tmux send-keys -t "$claude_pane" Enter
~~~

The supervisor is not pretending both agents expose the same API. It only requires the same tiny contract:

- a stable pane;
- one scoped input;
- a result file;
- a status file.

## Observe without guessing

The screen remains useful for live inspection:

~~~bash
tmux list-panes -t agent-lab:0 \
  -F '#{pane_id} #{pane_current_command} #{pane_dead}'

tmux capture-pane -p -J -S -80 -t "$codex_pane"
tmux capture-pane -p -J -S -80 -t "$claude_pane"
~~~

But completion should come from the status files, not from trying to recognize a prompt visually:

~~~bash
test -f "$lab_root/codex.status" && cat "$lab_root/codex.status"
test -f "$lab_root/claude.status" && cat "$lab_root/claude.status"
~~~

In the test, Claude exceeded an initial budget of 0.05 USD. The supervisor observed the non-zero result, raised the ceiling to 0.25 USD, and retried only that worker. A failed worker did not take the rest of the system down.

That failure was more informative than a clean demo. A useful orchestrator needs <code>FAILED</code>, <code>BLOCKED</code>, and <code>TIMEOUT</code>, not just a cheerful success path.

## Hand work from one agent to another

Once both workers finish, the supervisor can build a new prompt from their artifacts:

~~~bash
{
  echo "Synthesize these two worker reports into a minimal state machine."
  echo
  echo "CODEX WORKER:"
  cat "$lab_root/codex.out"
  echo
  echo "CLAUDE WORKER:"
  cat "$lab_root/claude.out"
} > "$lab_root/handoff.txt"

tmux send-keys -l -t "$codex_pane" \
  "codex exec --ephemeral --sandbox read-only \
  --skip-git-repo-check -C '$lab_root' \
  - < '$lab_root/handoff.txt'"
tmux send-keys -t "$codex_pane" Enter
~~~

The resulting protocol was:

> READY → INPUT → RUNNING → OUTPUT → DONE → READY<br>
> RUNNING → BLOCKED | TIMEOUT → DONE

That is the moment this becomes orchestration rather than two terminals running at once: the supervisor observes results, makes a routing decision, and creates the next task.

## Four rules that keep it sane

**Use stable pane IDs.** Capture <code>#{pane_id}</code> when spawning a worker and verify it before every send.

**Do not parse the screen as an API.** Capture-pane is excellent for humans and diagnostics. Machine state belongs in explicit result, status, and sentinel files.

**Isolate writers.** Read-only research workers can share a directory. Agents editing code should receive separate git worktrees and be integrated deliberately.

**Bound everything.** Give each task a wall-clock deadline, an inactivity timeout, a permission policy, and — where supported — a cost ceiling.

Also remember that captured panes may contain source code, credentials, or command history. The supervisor is part of the security boundary.

## Where this stops being enough

tmux is a good orchestrator for one machine, a handful of workers, and a human who wants to see what is happening. It is not a distributed queue, a durable database, or a cluster scheduler.

That is a feature. Start with the thing you can understand. Add infrastructure only when a real failure demands it.

tmux is not the intelligence of the system. It is simply the table where the intelligences sit.
