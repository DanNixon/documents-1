Maintenance in the Release Period
=================================

The maintenance period starts as soon as the Beta test period for the current release starts.  However during that time there are some tasks that will always take precendence over any work on maintenance tasks.

1. Urgent Bug fixes or vital improvments for the current release
2. Testing PR's or manual unscripted testing for the current release
3. Encouraging and working with scientists on Beta testing


Maintenance tasks for 3.14
==========================

Highest priority
----------------

1. **Look over tickets (assigned and created by you) and close invalid ones (everybody) and [stale branches](https://github.com/mantidproject/mantid/branches/stale)**
1. C++14 on all platforms
   1. Move to MSVS17 (Moore)
   1. SCL on rhel7 (Peterson)
      1. ~~[devtoolset-7](https://www.softwarecollections.org/en/scls/rhscl/devtoolset-7/) on RHEL 7 and other variants [PR22864](https://github.com/mantidproject/mantid/pull/22864)~~
      1. ~~Fix GCC 7 compiler warnings~~
   1. Update developer documentation
1. ~~Enable auto deployment of developer docs (Gigg/Peterson)~~ [done](http://builds.mantidproject.org/view/All/job/developer_site/5668/)
1. More installers
    1. Parallel python3 release for ubuntu 16.04 and 18.04 (Whitfield)
    2. Install workbench in `nightly` for rhel7 (Peterson) *actual work for next release*
3. Reducing static analysis issues that are on every pull request
   1. Move `clang-format` to version 5 (Moore)
   1. [cppcheck 1.84](http://builds.mantidproject.org/job/master_cppcheck/), upgrade version, move from jenkins into git
   2. [python3-flake8](http://builds.mantidproject.org/job/master_flake8_python3/) prioritizing complexity issues (issue [here](https://github.com/mantidproject/mantid/issues/20508))
   3. [pylint](http://builds.mantidproject.org/view/Static%20Analysis/job/master_pylint/) - need to review scope
   3. [clang-tidy](http://builds.mantidproject.org/view/Static%20Analysis/job/clang_tidy/)
1. Extract performance tests build configuration into a script in the repository (Nixon)
1. Update list of supported os on [download site](http://download.mantidproject.org/)
1. Move to El Capitan and drop support Yosemitte (Hahn)
1. ~~Common solution for ignoring `parentheses-equality` and `inconsistent-missing-override` warnings for Clang as well as GCC. (Jackson)~~
1. `Kernel::make_unique` -> `std::make_unique` and remove former.


Pool
----

Start from the top of the list

1. Check for slow unit tests
1. Reducing static analysis issues (discus stewards and soft limits)
    1. [coverity](https://scan.coverity.com/projects/335)
    3. [clang-tidy](http://builds.mantidproject.org/view/Static%20Analysis/job/clang_tidy/)
    1. [pylint](http://builds.mantidproject.org/job/master_pylint/)
    2. [address-sanitizer](http://builds.mantidproject.org/view/Static%20Analysis/job/address_sanitizer/)
    2. [valgrind](http://builds.mantidproject.org/view/Valgrind/job/valgrind_core_packages/) (is currently only kernel and geometry)
1. enable warnings and fix issues
   1. [-Wdouble-promotion](https://gist.github.com/quantumsteve/38c7be4a5606edecb223) (GCC only)
   1. [-Wfloat-equal](https://gist.github.com/quantumsteve/05b55c0743030b8c439d) (GCC and clang)
   1. create a common `almost_equals` function in Kernel [see this](http://en.cppreference.com/w/cpp/types/numeric_limits/epsilon).
7. Change tests of `CurveFitting` "functions" to be actual unit tests [#16267](https://github.com/mantidproject/mantid/issues/16267) (Gemma)
1. Remove finders that exist in standard cmake 3.5
15. Remove uses of strcpy, sprintf, etc. [See ParaView-developers thread ](http://public.kitware.com/pipermail/paraview-developers/2017-April/005276.html)
1. Investigate overhead from logging [#20493](https://github.com/mantidproject/mantid/issues/20493)
1. Editing actual variable and class names - investigate the discrepancy of our code with that in [C++ coding standards](http://www.mantidproject.org/C%2B%2B_Coding_Standards) and not covered by `clang-format`, max 1 days effort
1. Investigate breaking issues with updated dependencies: iPython 5.0 on mac
1084. Compilation times of components of the [pipeline build for master nightly](http://builds.mantidproject.org/view/Master%20Pipeline/) in static analysis tab (Ross)
1. Look at addressing issues shown up by [clang-tidy](http://builds.mantidproject.org/view/Static%20Analysis/job/clang_tidy). Someone needs to look through the issues and first prioritize what we look at, potentially see what the `autofix` can do for us. (Steve)
   1.  Split [performance-unnecessary-value-param](https://github.com/mantidproject/mantid/tree/performance-unnecessary-value-param) branch into smaller pieces and assign to pool
   1. Check whether it's acceptable to pass a pointer (nullable) or a reference (not null) instead of a `shared_ptr`.
   2. Smaller checks that could be updated in a single PR.
1. Change developer rpm to follow [packaging guidelines](https://fedoraproject.org/wiki/Packaging:PkgConfigBuildRequires)

For another release
-------------------

1. Remove workarounds for RHEL5/6 scattered around the code (mainly PythonInterface layer). (Martyn) *still needs to be done?*
11. Stop using classes and member function removed in C++17.
    1. MSVC update 3 introduces [macros for fine-grained control](https://blogs.msdn.microsoft.com/vcblog/2016/08/12/stl-fixes-in-vs-2015-update-3/): `_HAS_AUTO_PTR_ETC`, `_HAS_OLD_IOSTREAMS_MEMBERS`, `_HAS_FUNCTION_ASSIGN`, `_HAS_TR1_NAMESPACE`, `_HAS_IDENTITY_STRUCT`
    2. See which ones we can turn off now.
    3. Identify functions and classes with deprecated code.
    4. example: we currently use `std::auto_ptr` with `boost::python`.
1. Investigate and distribute rewrite/refactor nexus algorithms - [#12591](http://github.com/mantidproject/mantid/issues/12591)  (Martyn)
2. Harmonizing external contributions with the rest of mantid (e.g. PSI subpackage) [#12630](https://github.com/mantidproject/mantid/issues/12630) (Pete)
3. Rework/clean up cmake as a whole
4. Making ANN an ExternalProject
1. [Boost 1.63](http://www.boost.org/users/history/version_1_63_0.html) has some nice improvements to `boost::python` - not available everywhere
   1. Added (basic) support for C++11 (std::shared_ptr, std::unique_ptr) *isn't this done?*
   2. Incorporated an extension API to wrap NumPy
   3. Would require building packages for Linux distributions with older version.
13. [Add Labels to unit tests](https://github.com/mantidproject/mantid/issues/17453)
14. [Improve ctest support with multi-configuration generators](https://github.com/mantidproject/mantid/issues/19303)
16. [Update gSOAP by using the system package or making it an external project](https://github.com/mantidproject/mantid/issues/19433)
10. Clang working more places (windows [#20492](https://github.com/mantidproject/mantid/issues/20492), neutronatom [#11542](https://github.com/mantidproject/mantid/issues/11542), [#9267](https://github.com/mantidproject/mantid/issues/9267), [#7565](https://github.com/mantidproject/mantid/issues/7565), [#5670](https://github.com/mantidproject/mantid/issues/5670), and a singleton stopping initializing python [#15293](https://github.com/mantidproject/mantid/issues/15293))
10. Remove uses of the deprecated [Q_FOREACH macro](https://www.kdab.com/goodbye-q_foreach/) (and `foreach`)
1. Restructuring `Framework` (and whole package structure) to make building and exporting classes easier
1. all systemtests at least work on one platform [skipped system tests](http://developer.mantidproject.org/systemtests/) [#12615](https://github.com/mantidproject/mantid/issues/12615) (Pete)
   1. Design document for next iteration of testing (splitting small and big system tests, select where they run) - Pete
1. Document how to [turn things on in `cmake`](https://blog.kitware.com/static-checks-with-cmake-cdash-iwyu-clang-tidy-lwyu-cpplint-and-cppcheck/) (Hahn)

Converted to actual tickets during a release
--------------------------------------------

1. Documentation
   1. Change citations to new plugin [#21750](https://github.com/mantidproject/mantid/pull/21750)(Peterson)
   1. Move training to user `.rst` and update for Python 3 compatability(Savici)
   1. Add document for migrating scripts to python2/3 compatible (Whitfield)
1. Add `f2py` code to the builds - this is an ongoing process, only complex items remain (translating fortran to python and effectively support as python)
1. Proper rpm and deb packages without cpack ([rpm and cmake](https://fedoraproject.org/wiki/Packaging:Cmake))
