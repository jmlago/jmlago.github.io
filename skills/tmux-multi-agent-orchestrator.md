# tmux Is the Smallest Multi-Agent Framework I Could Build

A persistent GPT-5.6-Sol debated Fable 5 inside two panes. I routed the messages and judged the result.

## The correction

My first version of this experiment launched `codex exec` and `claude -p`, waited for two files, then exited. It worked, but it missed the interesting part.

Coding agents already have interactive sessions. They remember earlier turns, accept follow-ups, expose their work on screen, and can be interrupted by a human. Throwing that away turns them into expensive shell commands.

So the better question is:

> What if the smallest multi-agent runtime is just persistent TUIs inside tmux?

The supervisor keeps the pane IDs, sends messages with `tmux send-keys`, reads the visible state with `capture-pane`, and routes one agent's answer to another. A human can attach to the same session at any moment. No agent SDK is required.

## Two agents that stay alive

I created an empty scratch directory and one wide tmux window with two panes:

~~~bash
lab_root=$(mktemp -d -t agent-lab.XXXXXX)
mkdir -p "$lab_root"/{codex,claude}

tmux new-session -d -s agent-lab -n agents -x 220 -y 52 \
  -c "$lab_root/codex"
tmux split-window -h -t agent-lab:agents -c "$lab_root/claude"
tmux select-layout -t agent-lab:agents even-horizontal

codex_pane=$(tmux display-message -p -t agent-lab:agents.0 '#{pane_id}')
claude_pane=$(tmux display-message -p -t agent-lab:agents.1 '#{pane_id}')
~~~

Then I launched the normal interactive clients, not their print modes:

~~~bash
tmux send-keys -l -t "$codex_pane" \
  'codex --dangerously-bypass-approvals-and-sandbox'
tmux send-keys -t "$codex_pane" Enter

tmux send-keys -l -t "$claude_pane" \
  'claude --model fable --dangerously-skip-permissions --name fable-worker'
tmux send-keys -t "$claude_pane" Enter
~~~

Those flags are intentionally dangerous. They remove local approval and sandbox boundaries; an empty working directory is tidiness, not isolation. I used them for a disposable test with harmless prompts. For real code, use safer permissions or put the whole lab inside an actual container.

After accepting each client's workspace prompt, both agents remained alive. I could watch them with:

~~~bash
tmux attach -t agent-lab

tmux capture-pane -p -J -S -80 -t "$codex_pane"
tmux capture-pane -p -J -S -80 -t "$claude_pane"
~~~

Detach with `Ctrl-b d`; the conversations continue. The interactive clients also persist their own session history, so keeping the pane alive is the fast path, not the only recovery path.

## tmux is the bus

The supervisor only needs two operations:

~~~bash
send() {
  local pane=$1
  shift
  tmux send-keys -l -t "$pane" "$*"
  sleep 1
  tmux send-keys -t "$pane" Enter
}

observe() {
  tmux capture-pane -p -J -S -80 -t "$1"
}
~~~

That tiny adapter is deliberately imperfect. During the experiment, Codex's paste protection once left a long prompt staged instead of submitted. The supervisor saw it in `capture-pane` and sent Enter again. This is a useful boundary: tmux transports keystrokes, but a full-screen TUI is not a machine protocol.

The eventual design avoids giant pasted prompts. It puts the task in a file and sends the agent a short nudge containing the path.

## The debate

I ran this on 7 August 2026 with tmux 3.6a, Codex CLI 0.146.0, and Claude Code 2.1.220. The two workers were GPT-5.6-Sol at max effort and Fable 5 at xhigh effort. I gave them the same question independently:

> What is the simplest genuinely useful multi-agent framework for local coding agents? One machine, persistent interactive sessions, human attachment, and no external service.

I then copied each answer into the other session. Neither agent was restarted between rounds.

### Round one: independent positions

They converged immediately on the important distinction.

Fable called tmux “only the transport substrate” and proposed a shared filesystem protocol plus a small dispatcher. GPT-5.6-Sol also made tmux the substrate and called the files—not tmux—the coordination framework.

Both rejected databases, brokers, networking, and agent graph libraries for this scale. Both made the attached human a first-class participant.

### Round two: attack the weak point

The disagreement was about how much distributed-systems machinery survives on one laptop.

Fable attacked locks and atomic claims: with one dispatcher, there is no concurrent assignment race. Lock files introduce stale-lock recovery, exactly the sort of framework growth we were trying to avoid.

GPT-5.6-Sol attacked the phrase “delivery semantics for free.” Atomic rename makes a transition durable, but it does not provide acknowledgement, deduplication, crash recovery, or proof that `send-keys` reached an idle prompt.

That criticism improved both designs. They dropped `STATE.md`, duplicate status logs, locks, and automatic retries. The directory layout itself became the state machine.

### Round three: converge

Fable named the result `tmuxq`:

~~~text
queue/<id>.md
    → active/<agent>/<id>.md
    → done/<id>.md
~~~

Each task contains an objective, scope, and expected output. Moving the file is the state transition. The dispatcher moves it to one agent's active directory and sends a one-line nudge. The agent appends its result and moves the file to `done/`.

GPT-5.6-Sol proposed the same state machine using filename suffixes instead of directories, plus a dispatcher under fifty lines of shell. Its unresolved problem was honest: `send-keys` preserves a transparent interactive session, but cannot prove that the agent is sitting at a safe prompt.

### Round four: sign or dissent

I sent each final proposal to the other and allowed only `SIGN` or one indispensable change.

GPT-5.6-Sol replied:

> SIGN

Fable accepted the design with one condition:

> Agents must re-scan their active tasks at the end of every turn. Nudges should reduce latency, not carry correctness.

## The verdict

I accept that condition. The smallest useful design is:

1. One tmux session with persistent, named agent panes.
2. Stable pane IDs captured when workers start.
3. Files moving through `queue/`, `active/<agent>/`, and `done/` as the only durable task state.
4. A tiny dispatcher that assigns work and sends short path-based nudges.
5. Cooperative re-checking after each agent turn.
6. A human supervisor for inspection, arbitration, and manual recovery.

No locks. No database. No duplicate state file. No automatic retry engine. If a task stalls, the human can attach, see the conversation, and move it back to the queue.

There are really two useful layers here. For a live, supervised experiment, tmux plus `send-keys` is already orchestration. `tmuxq` is the smallest extra layer that makes the work crash-legible and recoverable.

## The topology is not fixed

The debate used two peers and a judge, but the same primitives can express a supervisor with many workers, a research-to-implementation pipeline, an author-reviewer loop, a hierarchy of supervisors, or a pool of temporary specialists.

tmux does not decide who talks to whom. That topology is just routing policy. The panes are processes, pane IDs are addresses, `send-keys` is transport, files are durable state, and `attach` is the debugger.

For one machine and a handful of coding agents, it is difficult to make a framework smaller without making the failure modes invisible.

> tmux is not the intelligence of the system. It is the room where the intelligences keep talking.
