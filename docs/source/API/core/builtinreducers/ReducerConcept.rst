.. _ReducerConcept:

``ReducerConcept``
==================

The concept of a Reducer is the abstraction that defines the "how" a "Reduction" is performed during the parallel reduce execution pattern.  The abstraction of "what" is given as a template parameter and corresponds to the "what" that is being reduced in the :ref:`parallel_reduce <parallel_reduce()>`. operation.  This page describes the definitions and functions expected from a Reducer with a hypothetical 'Reducer' class definition.  A brief description of built-in reducers is also included. 

.. admonition:: WARNING
  :class: attention

  This class changed its behavior in Kokkos 4.0:

    * Previously this class was super lame
    * Now this class is super cool
       

.. role:: cpp(code)
   :language: cpp

Header File: ``Kokkos_Core.hpp``

Usage: 

.. code-block:: cpp

  T result;
  parallel_reduce(N,Functor,ReducerConcept<T>(result));


Synopsis
--------
.. code-block:: cpp

  class Reducer {
   public:
     //Required for Concept
     typedef Reducer reducer;
     typedef ... value_type;
     typedef Kokkos::View<value_type, ... > result_view_type;
     
     KOKKOS_INLINE_FUNCTION
     void join(value_type& dest, const value_type& src)  const

     KOKKOS_INLINE_FUNCTION
     void join(volatile value_type& dest, const volatile value_type& src) const;

     KOKKOS_INLINE_FUNCTION
     void init( value_type& val)  const;

     KOKKOS_INLINE_FUNCTION
     value_type& reference() const;

     KOKKOS_INLINE_FUNCTION
     result_view_type view() const;

     
     //Part of Build-In reducers for Kokkos
     KOKKOS_INLINE_FUNCTION
     Reducer(value_type& value_);

     KOKKOS_INLINE_FUNCTION
     Reducer(const result_view_type& value_);
  };

Class Interface
---------------

.. cpp:class:: Reducer

  A hypothetical ``Reducer`` class definition. Can be class template if required.

  .. rubric:: Required public nested types
  .. cpp:type:: reducer

    The self type.

    .. admonition:: Deprecation warning
      :class: warning

      Deprecated in Kokkos 3.7, removed in Kokkos 4.0

  .. cpp:type:: value_type

    The reduction scalar type.

  .. cpp:type:: result_view_type

    A cpp:type:`Kokkos::View` referencing where the reduction result should be placed. Can be an unmanaged view of a scalar or complex datatype (class or struct).  Unmanaged views must specify the same memory space where the referenced scalar (or complex datatype) resides.

  .. rubric:: Constructors

  Constructors are not part of the concept. A custom reducer can have complex custom constructors. All Build-In reducers in Kokkos have the following two constructors:

  .. cpp:function:: Reducer(value_type& result)

    :param result: Result location

    Constructs a reducer which references a local variable as its result location.  

  .. cpp:function:: Reducer(const result_view_type result)

    :param result_view_type: View to store the results

    Constructs a reducer which references a specific view as its result location.

  .. rubric:: Required member functions

  .. cpp:function:: void join(value_type& dest, const value_type& src)  const;

    :param dest: fist value to combine
    :param src: second value to combine

    .. admonition:: Addition
      :class: tip
      
      Since Kokkos 4.0 
     
    Combine ``src`` into ``dest``. For example, :cpp:type:`Kokkos::Sum` performs ``dest+=src;``. 

  .. cpp:function:: void join(volatile value_type& dest, const volatile value_type& src) const;

    :param dest: fist value to combine
    :param src: second value to combine

    .. admonition:: Removal
      :class: attention
      
      Until Kokkos 4.0 
     
    Combine ``src`` into ``dest``. For example, :cpp:type:`Kokkos::Sum` performs ``dest+=src;``. 

  .. cpp:function:: void init( value_type& val)  const;
   
    :param val: value to be intialized

    Assigns ``val` with appropriate identity element for the operation. For example, :cpp:type:`Kokkos::Sum` assigns ``val = 0;``, but :cpp:type:`Kokkos::Prod` assigns ``val = 1;``

  .. cpp:function:: value_type & reference() const;
    
    :return: a reference to the result place.

  .. cpp:function:: result_view_type view() const;
  
    :return: a view of the result place. 

Built-In Reducers
^^^^^^^^^^^^^^^^^

Kokkos provides a number of built-in reducers that automatically work with the intrinsic C++ types as well as :cpp:type:`Kokkos::complex`.  In order to use a Built-in reducer with a custom type, a template specialization of :cpp:type:`Kokkos::reduction_identity<CustomType>` must be defined.  A simple example is shown below and more information can be found under :ref:`Custom Reductions <Custom-Reductions>`.
 * :ref:`Kokkos::BAnd <BAnd>`
 * :ref:`Kokkos::BOr <BOr>`
 * :ref:`Kokkos::LAnd <LAnd>`
 * :ref:`Kokkos::LOr <LOr>`
 * :ref:`Kokkos::Max <Max>`
 * :ref:`Kokkos::MaxLoc <MaxLoc>`
 * :ref:`Kokkos::Min <Min>`
 * :ref:`Kokkos::MinLoc <MinLoc>`
 * :ref:`Kokkos::MinMax <MinMax>`
 * :ref:`Kokkos::MinMaxLoc <MinMaxLoc>`
 * :ref:`Kokkos::Prod <Prod>`
 * :ref:`Kokkos::Sum <Sum>`

Examples
--------

.. code-block:: cpp

   #include<Kokkos_Core.hpp>
   
   int main(int argc, char* argv[]) {

     long N = argc>1 ? atoi(argv[1]):100; 
     long result;
     Kokkos::parallel_reduce("ReduceSum: ", N, KOKKOS_LAMBDA (const int i, long& lval) {
       lval += i;
     },Sum<long>(result));

     printf("Result: %l Expected: %l\n",result,N*(N-1)/2);
   }
