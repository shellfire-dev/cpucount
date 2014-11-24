# `cpucount`: functions module for [shellfire]

This module provides utility functions for counting cpus to a [shellfire] application. It is deliberately separate to the [core] module as it is not likely to be frequently needed.

An example user is the [cmake-scripted] tool.

## Overview

To find the number of hyperthreaded CPUs on a system:
```bash
local cpucount_discoveredNumberOfCpus
cpucount_findNumberOfHyperthreadedCpus
echo "Number of CPUs: $cpucount_discoveredNumberOfCpu"
```

```bash
local cpucount_makeJobs
local cpucount_makeLoadAverage
cpucount_computeMakeJobsAndLoadAverage 0 75
echo "Number of make jobs: $cpucount_makeJobs"
echo "Make load average: $cpucount_makeLoadAverage"
```

## Importing

To import this module, add a git submodule to your repository. From the root of your git repository in the terminal, type:-
```bash
mkdir -p lib/shellfire
cd lib/shellfire
git submodule add "https://github.com/shellfire-dev/cpucount.git"
cd -
git submodule init --update
```

You may need to change the url `https://github.com/shellfire-dev/cpucount.git"` above if using a fork.

You will also need to add paths - include the module [paths.d].

## Namespace `cpucount`

### To use in code

If calling from another [shellfire] module, add to your shell code the line
```bash
core_usesIn configure
```
in the global scope (ie outside of any functions). A good convention is to put it above any function that depends on functions in this module. If using it directly in a program, put this line _inside_ the `_program()` function:-

```bash
_program()
{
	core_usesIn cpucount
	…
}
```

### Functions

***
#### `cpucount_findNumberOfHyperthreadedCpus`

|Parameter|Value|Optional|
|---------|-----|--------|

_Takes no parameters._

_Sets `cpucount_discoveredNumberOfCpus` to the number of hyperthreaded CPUs._

***
#### `cpucount_computeMakeJobsAndLoadAverage`

|Parameter|Value|Optional|
|---------|-----|--------|
|`configuredCpuCount`|Integer. Set to `0` to use the value of `cpucount_discoveredNumberOfCpus`. Set to a negative value to use `cpucount_discoveredNumberOfCpus` reduced by the magnitude of the negative value.|_No_
|`configuredLoadAverageCorrection`|Integer percentage between 0 and 100. Used to reduce the load average by this percent. |_No_|

_Sets `cpucount_makeJobs` to the number of make jobs and `cpucount_makeLoadAverage` to the make load average to use._

The `configuredLoadAverageCorrection` is used to overcome the shell's lack of floating point maths. For instance, if the uncorrected load average would be 4.00, using a `configuredLoadAverageCorrection` of 75 will result in `cpucount_makeLoadAverage` being `3.25`. If `cpucount_makeLoadAverage` would be `0.00` then it is forced to `0.01`.


[shellfire]: https://github.com/shellfire-dev "shellfire homepage"
[core]: https://github.com/shellfire-dev/core "shellfire core module homepage"
[paths.d]: https://github.com/shellfire-dev/paths.d "paths.d shellfire module homepage"
[cmake-scripted]: https://github.com/raphaelcohn/cmake-scripted "CMake scripted homepage"
