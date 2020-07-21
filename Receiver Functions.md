# Receiver Functions

A Receiver Function takes an object known as the 'receiver' and a value to apply to the receiver.
The function mutates the receiver using the value.

:warning: Since the receiver is able to be mutated - care should be taken not to make unwanted references to the object - that may mutate the receiver in an unexpected way!

Since the value is the thing generally being piped through a pipe, this should be the last partial in a Receiver Function.

E.g.
