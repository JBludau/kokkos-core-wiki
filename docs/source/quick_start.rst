Quick Start
============

This guide is intended to jump start new Kokkos users (and beginners, in particular).  Kokkos Core is the foundation for all other Kokkos Ecosystem projects, and is readily usable in your own for performance portability.


Download Latest and Build 
-----------------------------

.. note::

  Please become familiar with `Kokkos Requirements <https://kokkos.org/kokkos-core-wiki/requirements.html>`_, and verify that your machine has all necessary compilers, backend SDK and build system components.


..
 Nota bene:  the link for "Latest" should be stable from one release to the next, but check periodically to be sure 
..

:bdg-link-primary:`Latest Release <https://github.com/kokkos/kokkos/releases/latest>`

.. code-block:: sh
  
  # Uncomment according to the type of file you've downloaded (zip or tar)
  unzip kokkos-x.y.z.zip 
  # tar -xzf kokkos-x.y.z.tar.gz
  cd kokkos-x.y.z
  mkdir build
  cd build


Basic Configuration
-------------------

.. note::

  You can create small shell scripts to manage and experiment with configuration details, following the GPU microarchitecture-appropriate examples below.  Upon successful configuration, ``make -j`` to build, and ``make install`` to install.



OpenMP (CPU Parallelism)
~~~~~~~~~~~~~~~~~~~~~~~~

.. code-block:: sh

  cmake \
   -DKokkos_ENABLE_OPENMP=ON \
   -DCMAKE_BUILD_TYPE=Release ..


.. note::

  Kokkos will attempt to autodetect GPU microarchitecture, but it is also possible to specify the desired `GPU architecture <https://kokkos.org/kokkos-core-wiki/keywords.html#gpu-architectures>`_.  In scenarios where a backend (e.g., CUDA, HIP) is enabled, Kokkos will execute serially on the "host" (CPU).  

CUDA (CPU and GPU)
~~~~~~~~~~~~~~~~~~

.. code-block:: sh

  cmake \
    -DKokkos_ENABLE_CUDA=ON \
    -DCMAKE_BUILD_TYPE=Release ..


HIP (CPU and GPU)
~~~~~~~~~~~~~~~~~

.. code-block:: sh

  cmake \
    -DKokkos_ENABLE_HIP=ON \
    -DCMAKE_CXX_COMPILER=hipcc \
    -DCMAKE_BUILD_TYPE=Release ..


Basic Configuration Examples for Spack Builds
----------------------------------------------

|br|
Diverse Kokkos variants can be built / installed via Spack.  You will need to select a variant for the desired backend, and appropriate GPU microarchitecture.  To explore the range of variants for a package, ``spack info kokkos``, ``spack info trilinos``, etc.  Please see basic `Spack installation  <https://spack.readthedocs.io/en/latest/getting_started.html>`_ instructions if you're new to the package manager.
|br|
|br|


.. note::

  Before installing, you can ``spack spec``  variants to verify the build type.

Serial
~~~~~~~

.. code-block:: sh

  spack spec kokkos@4.2 %gcc@10.3.0 +serial cxxstd=20

OpenMP
~~~~~~

.. code-block:: sh

  spack spec kokkos@4.2 %gcc@10.3.0 +openmp cxxstd=20


CUDA
~~~~

.. code-block:: sh
  
  spack spec / install kokkos@4.2 %gcc@12.2.0 +cuda cuda_arch=70 cxxstd=20 +cuda_relocatable_device_code


HIP
~~~

.. code-block:: sh

  spack spec / install kokkos@4.2 %gcc@12.2.0 +rocm amdgpu_target=gfx90a cxxstd=20


Building and Linking a Kokkos "Hello World"
-------------------------------------------

.. note::

  You will need to set ``Kokkos_ROOT``, and also the root directory for you target backend SDK (i.e., ``CUDA_ROOT``, ``ROCM_PATH``).  Please see `Build, Install and Use <https://kokkos.org/kokkos-core-wiki/building.html>`_ for additional details.

|br|

.. code-block:: sh

  git clone https://github.com/ajpowelsnl/View
  cd View
  mkdir build
  cd build
  cmake ../


Get Involved!
-------------

Join the Kokkos Community
~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Participate in the lively virtual discussion on the `Kokkos Slack Channel <https://kokkosteam.slack.com>`_.  Here are `sign up details <https://kokkos.org/kokkos-core-wiki/faq.html#faq>`_.  Joining Kokkos Slack is the on ramp for becoming a project contributor.
|br|

Acclerate learning
~~~~~~~~~~~~~~~~~~~~

Take a deeper dive into Kokkos with over 16 hours of `Tutorials <https://github.com/kokkos/kokkos-tutorials>`_ and `Recorded Lectures <https://github.com/kokkos/kokkos-tutorials/wiki/Kokkos-Lecture-Series>`_.  For in-house workshops and training, please get in touch via Slack (below).
|br|

Come to Release Demos
~~~~~~~~~~~~~~~~~~~~~~~

You have a standing invitation to attend our approximately quarterly release demos to hear what's new, what's gone, and where we're heading in the future.
|br|

Attend Kokkos Users' Group Meetings
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Please join us in our annual Kokkos Users' Group Meeting, where we present recent Kokkos work, and you showcase how you use Kokkos.  It's a great opportunity to build community and grow collaboration.
|br|


..
  *TODO*
     - Integrate (merged) Quick Start with Cédric's PR:  https://github.com/kokkos/kokkos/pull/6796
     - Ongoing reconciling with the Julien B. / KUG23- initiated discussion:  https://github.com/kokkos/internal-documents/pull/19
     - Add `git submodule` "how to" for Kokkos
     - Add Quick Start to main Kokkos page, such that it is the first thing you encounter on the landing page (kokkos.org)
     - In V2, put the recipes for the different backends on different pages
     - Julien B. suggested using github templates for the View "Hello World" example
     - Nic M.:  CUDA as a CMake language example (using View): cmake -S . -B build -DKokkos_ENABLE_CUDA=ON CMAKE_CUDA_COMPILER=nvcc Kokkos_ENABLE_COMPILE_AS_CMAKE_LANGUAGE=ON [-DCMAKE_BUILD_TYPE=Release]


..

.. |br| raw:: html

      <br>

