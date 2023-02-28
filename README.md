# EVENODD Erasure Code (EOEC) in C

A C implementation of the Even-Odd Erasure Code (EOEC) that uses io_uring and bstring libraries for better performance.

## Introduction

EOEC is an erasure code that provides fault tolerance in distributed storage systems. This implementation of EOEC uses io_uring for efficient and scalable I/O operations and bstring for efficient string manipulation.

EVENODD[1] is defined by a prime number `p`. It can encode an array of size $(p-1) \times p$ into an array of size $(p-1) \times (p+2)$. It can accommodate any two errors, i.e. any one or two columns of data lost can be recovered from the remaining data. We call the first p columns "data columns" and the last two columns "parity columns". The 0th parity column is called the "row parity" and the 1st parity column is called the "diagonal parity".

This implementation of EOEC supports three operations:

- `write`: encode a file and write it to the storage system
- `read`: decode a file from the storage system and save it to a file
- `repair`: repair a file from erasures by regenerating the missing data blocks

## Requirements

To build and run EOEC, you need:

- A C compiler that supports C99
  
- xmake
  
- io_uring library
  
- bstring library
  

## Build Instructions

1. Clone this repository
  
2. Build the project:
  
  ```shell
  make
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

## References

[1] M. Blaum, J. Brady, J. Bruck and Jai Menon, "EVENODD: an efficient scheme for tolerating double disk failures in RAID architectures," in IEEE Transactions on Computers, vol. 44, no. 2, pp. 192-202, Feb. 1995, doi: 10.1109/12.364531.

[2] [axboe/liburing](https://link.zhihu.com/?target=https%3A//github.com/axboe/liburing)

## License

This project is licensed under the MIT License. See the LICENSE file for details.
