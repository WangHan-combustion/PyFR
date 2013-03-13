===============
Developer Guide
===============

Introduction
------------

A detailed developer guide is provided below

Internal File Formats
---------------------

Mesh Format (.pyfrm)
~~~~~~~~~~~~~~~~~~~~

Solution Format (.pyfrs)
~~~~~~~~~~~~~~~~~~~~~~~~

Code
----

Backend Interface
~~~~~~~~~~~~~~~~~

All of the backends in PyFR implement a common interface.  This
interface is based around the `factory method pattern
<http://en.wikipedia.org/wiki/Factory_method_pattern>`_ and consists
of a ``Backend`` class (the factory) and a series of types (products
of the factory).  Broadly speaking these types can be placed into one
of three categories: matrices, kernels and queues.

All algorithms in PyFR are implemented according to the following
procedure.  Firstly a series of matrices are allocated and populated
with suitable initial values. Next a set of kernels are defined to
operate these matrices.  Then one or more queues are allocated to
execute these kernels.  Finally, these queues are populated with the
relevant kernels and run.  As an example of this process consider
matrix multiplication::

  import numpy as np

  # Take be to be a concrete Backend instance
  be = get_a_backend()

  # Allocate two 1024 by 1024 matrices filled with random numbers
  m1 = be.const_matrix(np.random.rand(1024, 1024))
  m2 = be.const_matrix(np.random.rand(1024, 1024))

  # Allocate a third empty matrix
  m3 = be.matrix(1024, 1024)

  # Prepare a kernel to multiply m1 by m2 putting the result in m3
  mmul = be.kernel('mul', m1, m2, out=m3)

  # Allocate a queue
  q = be.queue()

  # Populate the queue with `mmul` and run it
  q % [mmul]

.. autoclass:: pyfr.backends.base.Backend
    :members:

.. autoclass:: pyfr.backends.base.Matrix
    :members:
    :inherited-members:

.. autoclass:: pyfr.backends.base.ConstMatrix
    :members:
    :inherited-members:

.. autoclass:: pyfr.backends.base.SparseMatrix
    :members:
    :inherited-members:

.. autoclass:: pyfr.backends.base.MPIMatrix
    :members:
    :inherited-members:

.. autoclass:: pyfr.backends.base.MatrixBank()
    :members:
    :inherited-members:

.. autoclass:: pyfr.backends.base.View
    :members:

.. autoclass:: pyfr.backends.base.MPIView
    :members:

.. autoclass:: pyfr.backends.base.Kernel
    :members:

.. autoclass:: pyfr.backends.base.Queue
    :members: __lshift__, __mod__