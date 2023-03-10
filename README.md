# EVENODD Erasure Code (EOEC) in C

A C implementation of the Even-Odd Erasure Code (EOEC) that uses liburing[1] and bstring libraries for better performance.

## Introduction

EOEC is an erasure code that provides fault tolerance in distributed storage systems. This implementation of EOEC uses io_uring for efficient and scalable I/O operations and bstring for efficient string manipulation.

EVENODD[2] is defined by a prime number `p`. It can encode an array of size $(p-1) \times p$ into an array of size $(p-1) \times (p+2)$. It can accommodate any two errors, i.e. any one or two columns of data lost can be recovered from the remaining data. We call the first p columns "data columns" and the last two columns "parity columns". The 0th parity column is called the "row parity" and the 1st parity column is called the "diagonal parity".

This implementation of EOEC supports three operations:

- `write`: encode a file and write it to the storage system
- `read`: decode a file from the storage system and save it to a file
- `repair`: repair a file from erasures by regenerating the missing data blocks

## Requirements

To build and run EOEC, you need:

- A C compiler that supports C99
  
- xmake[3]
  
- liburing library
  
- bstring library
  

## Build Instructions

1. Install `liburing` library:
  
  ```shell
  git clone https://github.com/axboe/liburing.git;
  cd liburing;
  ./configure --cc=gcc --cxx=g++;
  make -j$(nproc);
  sudo make install;
  ```
  
2. Clone this repository:
  
  ```shell
  git clone https://github.com/PKUcoldkeyboard/EOEC.git;
  cd EOEC;
  ```
  
3. Build the project:
  
  ```shell
  xmake --root
  ```
  

## Usage

To encode a file and write it to the storage system:

```shell
./evenodd write <input_file> <p>
```

- `input_file`: the path to the input file
  
- `p`: parameter of EVENODD, a prime number
  

This will encode the input_file and write it into `p + 2` folders for disk_0, disk_1, ... disk_{p+1}，

example:

```shell
./evenodd write /path/to/file 5
```

To decode a file from the storage system and save it to a file:

```shell
./evenodd read <file_name> <save_as>
```

- `file_name`: The name of the file being read.
  
- `save_as`: the path to the output file
  

This will decode the file from the storage system and save it to the output file.

example:

```shell
./evenodd read /path/to/file tmp_file
```

To repair a file from erasures:

```shell
./evenodd repair <number_erasures> <idx> ...
```

- `number_erasures`: the number of errors
  
- `idx`: the IDs of the broken disks
  

This will regenerate the missing data blocks and repair the file. The repaired file will be written to the storage system.

example:

```shell
./evenodd repair 2 0 1
```

## Contributors

<a href="https://github.com/PKUcoldkeyboard/EOEC/graphs/contributors">
  <img src="https://contrib.rocks/image?repo=PKUcoldkeyboard/EOEC" />
</a>

Made with [contrib.rocks](https://contrib.rocks).

## References

[1] [axboe/liburing](https://github.com/axboe/liburing)

[2] M. Blaum, J. Brady, J. Bruck and Jai Menon, "EVENODD: an efficient scheme for tolerating double disk failures in RAID architectures," in IEEE Transactions on Computers, vol. 44, no. 2, pp. 192-202, Feb. 1995, doi: 10.1109/12.364531.

[3] [xmake-io/xmake](https://github.com/xmake-io/xmake)

## License

This project is licensed under the MIT License. See the LICENSE file for details.
