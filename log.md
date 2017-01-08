# 100 Days Of Code - Log

### Day 0: 6th January 2017

**Today's Progress**:
 - Rework the code to move to Python 3.5
 - Add initial docker integration
 - Use `asyncio` and `async/await` instead of Tornado primitives

**Thoughts:**
It was a difficult thing to decide how to revamp this project, specifically
because I wanted to experiment with async/await but there aren't any mature
web frameworks based solely on `asyncio`.

I also had to decide whether to use the Docker Python SDK directly or use
wrappers like `aiodocker`. Eventually, I ended up using the SDK directly
because the wrappers aren't well maintained right now.

It took some time to understand how `async/await` and `asyncio` works and how
are they different from each other. I'd recommend watching
[David Beazley's video](https://www.youtube.com/watch?v=lYe8W04ERnY) on the
topic.

**Link to work:**
 - [alia](https://github.com/dhruvbaldawa/alia)
 - [alia commit log](https://github.com/dhruvbaldawa/alia/commits/master?until=2017-01-07T12:00:00Z&since=2017-01-06T12:00:00Z)

### Day 1: 7th January 2017

**Today's progress**
 - Discarded the `Registry` way of handling containers and websockets
 - Added `WebsocketManager` to handle all the websockets.

**Thoughts:**
I have completely discarded `Registry` way of handling containers and
websockets. It led to too much mixing of concerns and I didn't want it to
handle everything from websocket connections to containers.

Instead, I  created a `WebsocketManager` to purely handle websocket connections.
I also moved to using explicit listeners to `WebsocketManager` so that data
from the websocket can be pushed to them, for example storing the logs in
database or sending the data to front-end websocket. This simplifies a lot
of that.

Another main reason of moving to `WebsocketManager` was because of
semantics of `aiohttp` and I need the session to be handled correctly. Since,
`aiohttp.ClientSession().ws_connect` uses a context manager and there is no way
to detach the session, the listener and receiver code should be inside the
context manager. Hence, the only sane thing to do was to include all of it into
a single task. Having a `WebsocketManager` that has this task and other
managing one place to keep all the websocket related objects and listeners,
seemed like the way to do it.

Most of the things worked at the end of the day, except for incomplete
destruction of pending tasks and open connections on quit and abort.

**Link to work:**
 - [alia](https://github.com/dhruvbaldawa/alia)
 - [alia commit log](https://github.com/dhruvbaldawa/alia/commits/master?until=2017-01-07T23:00:00Z&since=2017-01-06T23:00:00Z)

### Day 2: 8th January, 2017

**Today's progress**
 - Properly let all the tasks to die
 - Added client-side support by adding websocket proxies
 - Basic proof-of-concept is working

**Thoughts:**
The problem with tasks not exiting properly because I immediatedly stopped
the loop after canceling all the tasks. The coroutines didn't receive
`CancelledError` hence couldn't do proper cleanup operation. After adding a
small delay between canceling the tasks and stopping the loop fixed the issue.

Added front-end support and completed the websocket proxy, initial
proof-of-concept seems to be working fine.

Will plan what things and features to work on next.

**Link to work:**
 - [alia](https://github.com/dhruvbaldawa/alia)
 - [alia commit log](https://github.com/dhruvbaldawa/alia/commits/master?until=2017-01-08T12:00:00Z&since=2017-01-07T20:00:00Z)

## Template
### Day x: [DATE]

**Today's progress**
 - progress

**Thoughts:**
_thoughts_

**Link to work:**
 - [alia](https://github.com/dhruvbaldawa/alia)
 - [alia commit log](https://github.com/dhruvbaldawa/alia/commits/master?until=2017-01-07T12:00:00Z&since=2017-01-06T12:00:00Z)
