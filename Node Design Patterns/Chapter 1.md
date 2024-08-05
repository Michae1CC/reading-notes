- Node packages are intended to be very small and extremely reusable design packages.
- It is common to expose only one functionality, class or function for the simple fact that it provides a single clear entry point
- The unix operation systems has a sys call called fcntl to change an exisiting file descriptor to non-blocking
- Busy waiting - Keep iterating over non-blocking IO resources
- Event de multiplexing watches multiple resources and provides a blocking interface to read event from until a new message arrives from one of the resources. It also always us to handle multiple messages from the same thread

#### The Reactor Pattern
- The application generates new I/O operations by submitting a request to the event de-multiplexer. The app specifies the handler which invokes when the operation is complete
- When a set of operations complete, the event de-multiplexer pushes the events onto an event queue
- The handlers are then invoked with their corresponding events. 
- The handler, which is part of the event loop, gives back control to the event loop. These handlers can then submit new I/O operations to be completed causing new items to be added to the event loop.
- When all the items in the event queue are processed, the event loop blocks again on the event de-multiplexer which then triggers another cycle when a new event is available.



pg 10