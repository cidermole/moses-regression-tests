Moses regression tests
======================

This is the regression tests repository for the moses decoder [1,2].
Since it contains a couple of hundred MBs of data files, it is
distributed separately from the main sources.


Cloning into moses repo
-----------------------

To clone the regression tests into a git submodule (a versioned sub-folder)
of mosesdecoder, use the `git submodule update regtest` command as below:

	# fetch and change to the mosesdecoder repository
    # (but may also be your existing build setup)

    $ git clone https://github.com/moses-smt/mosesdecoder.git
    $ cd mosesdecoder

    # fetch the submodule https://github.com/moses-smt/moses-regression-tests.git

    $ git submodule init
    $ git submodule update regtest


Running a single test
---------------------

Update the mosesdecoder/regtest submodule as described above. Build moses executable
as usual in `bin/moses` (or update the path below).

Then, to run the test `phrase.basic-surface-only`:

    $ regression-testing/run-single-test.perl --test phrase.basic-surface-only --decoder bin/moses --data-dir regtest --test-dir regtest/tests

Successful output may look like this:

    [WARNING] Results directory for test=phrase.basic-surface-only could not be found.
    RESULTS AVAILABLE IN: /home/david/mmt/mmt-src-nosync/mosesdecoder/regtest/results/phrase.basic-surface-only/moses.v20151120-112316-david-at-20151120-115136

    test-name=SCORE_1       result=pass
    test-name=SCORE_2       result=pass
    test-name=SCORE_3       result=pass
    test-name=SCORE_4       result=pass
    test-name=SCORE_5       result=pass
    test-name=SCORE_6       result=pass
    test-name=TOTAL_WALLTIME        result=BASELINE=31, TEST=10       DELTA=-21      PCT CHANGE=-67.74
    test-name=TRANSLATION_1 result=pass
    test-name=TRANSLATION_3 result=pass
    test-name=TRANSLATION_4 result=pass
    test-name=TRANSLATION_5 result=pass
    test-name=TRANSLATION_6 result=pass

    TESTS PASSED=11
    TESTS FAILED=0

Failed output will instead end with something like:

    TESTS FAILED=1

    There were failures in this test run.  Please analyze the results carefully.

    FAILURE, for debugging, local moses.ini=/tmp/3FKPW2EAiG.ini

To debug, check the compressed moses stdout and stderr in the folder given after "RESULTS AVAILABLE IN".


Running all tests
-----------------

It is possible to include execution of all regression tests in the regular bjam build.
To do this, make sure the `regtest` submodule exists, as above. Then, use the bjam build switch `--with-regtest`:

    $ ./compile.sh --with-regtest=./regtest

This will compile Moses and run all regression tests. Currently, some tests may fail because they use parts of Moses not compiled by default. As of November 2015, these are IRSTLM and DALM, failing with the errors:

    Exception: IRSTLM0: Unknown argument
    Exception: Feature name DALM is not registered.

[1] http://www.statmt.org/moses/
[2] https://github.com/moses-smt/mosesdecoder

