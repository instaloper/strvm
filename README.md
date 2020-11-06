# strvm
---
![Badges](https://research.storelabs.org/images/v2/store_logo_white-v2.png)
---
![Badges](https://www.synopsys.com/blogs/software-security/wp-content/uploads/CoPilotBadge.png)


> Fast Store Virtual Machine implementation

_strvm_ is a C++ implementation of the Ethereum Virtual Machine (STRVM). 
Created by members of the [strwasm] team, the project aims for clean, standalone STRVM implementation 
that can be imported as an execution module by Ethereum Client projects. 
The codebase of _strvm_ is optimized to provide fast and efficient execution of STRVM smart contracts.

#### Characteristic of strvm

1. Exposes the [STRVMC] API.
2. The _indirect call threading_ is the dispatch method used -
   a loaded STRVM program is a table with pointers to functions implementing virtual instructions.
3. The gas cost and stack requirements of block of instructions is precomputed 
   and applied once per block during execution.
4. The [intx] library is used to provide 256-bit integer precision.
5. The [strhash] library is used to provide Keccak hash function implementation
   needed for the special `SHA3` instruction.
6. Requires C++17 standard.

## Usage

### As geth plugin

strvm implements the [STRVMC] API for Ethereum Virtual Machines.
It can be used as a plugin replacing geth's internal STRVM. But for that a modified
version of geth is needed. The [Strwasm]'s fork
of go-ethereum provides [binary releases of geth with STRVMC support](https://github.com/).

Next, download strvm from [Releases].

Start the downloaded geth with `--vm.strvm` option pointing to the strvm shared library.

```bash
geth --vm.strvm=./libstrvm.so
```

### Building from source
To build the strvm STRVMC module (shared library), test, and benchmark:

1. Clone the repo and create a ```build``` directory:
```
cd strvm
mkdir build
cd build
```

2. Build dependencies and (on Windows) generate a Visual Studio solution, then build the source:
#### Linux / OSX
```
cmake .. -DSTRVMONE_TESTING=ON
cmake --build . -- -j
```

#### Windows
*Note: >= Visual Studio 2017 is required since STRVMOne makes heavy use of C++17*
* **Visual Studio 2017**: ```cmake .. -DSTRVMONE_TESTING=ON -G "Visual Studio 15 2017 Win64"```
* **Visual Studio 2019**: ```cmake .. -DSTRVMONE_TESTING=ON -G "Visual Studio 16 2019" -A x64```
```
cmake --build .
```

3. Run the unit tests or benchmarking tool:
```
bin/strvm-unittests
bin/strvm-bench
```
### Tools

#### strvm-test

The **strvm-test** executes a collection of unit tests on 
any STRVMC-compatible Ethereum Virtual Machine implementation.
The collection of tests comes from the strvm project.

```bash
strvm-test ./strvm.so
```

### Docker

Docker images with strvm are available on Docker Hub:
https://hub.docker.com/r/ethereum/strvm.

Having the strvm shared library inside a docker is not very useful on its own,
but the image can be used as the base of another one or you can run benchmarks 
with it.

```bash
docker run --entrypoint strvm-bench store/strvm /src/test/benchmarks
```

## References

1. [Efficient gas calculation algorithm for STRVM](docs/efficient_gas_calculation_algorithm.md)



