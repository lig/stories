=======
 Usage
=======

Technically speaking, a story is a method of the class which calls
other methods of this class in special order.

The body of the story isn't executed directly.  It's used as a spec of
what should happen and when.

Stories called as usual methods.  First of all, it will build spec of
methods using its body.  Then it will call methods of this class
according to this spec.  This happens outside of the story body.  Each
method is called with special execution context.

Execution rules
===============

There are some rules on how stories are executed:

* Methods called in the order as they written in the story

* If the story calls another story in its body, methods of this
  sub-story add to the caller in the order they occur in sub-story
  body.

* Each story method should return an instance of ``Success``,
  ``Failure``, ``Result`` or ``Skip`` classes.

* If story method return ``Success`` execution of the whole story
  continues from the next step.

* Story method can use ``Success`` keyword arguments to set some
  context variables for future methods.  For example, if previous
  method return ``Success(foo="bar")``, current method can use
  ``self.ctx.foo`` to examine ``"bar"`` value.

* If story method return ``Failure``, the whole story considered
  failed.  Execution stops at this point.

* ``Failure`` of the sub-story will fail the whole story.

* If the story method return ``Result``, the whole story considered
  done.  The argument passed to the ``Result`` constructor will be the
  return value of the story call.

* The ``Result`` of the sub-story will be the result of the whole
  story.

* If sub-story method return ``Skip`` result, execution will be
  continued form the next method of the caller story.

* If the topmost story return ``Skip`` result, execution will end.