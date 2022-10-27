# Binary Streaming Reader

## Requirements

- PHP ^8.1

## Installation

Library is available as composer repository and can be installed using the 
following command in a root of your project.

- `composer require bic-engine/binary`

## Usage

### Creating Streams

```php
// 1) Stream from file:
$stream = new \Bic\Binary\FileStream('path/to/file.bin');


// 2) Stream from resource:
// 2.1) Do not close resource in object's destructor (by default)
// 2.2) Close resource stream in object's destructor
$resource = \fopen('path/to/file.bin', 'rb');

$stream = new \Bic\Binary\ResourceStream($resource, close: false);
$stream = new \Bic\Binary\ResourceStream($resource, close: true);


// 3) Typed stream:
// 3.1) With auto endianness (using OS settings)
// 3.2) With little endianness
// 3.3) With big endianness
$typed = new \Bic\Binary\TypedStream($stream, endianness: null);
$typed = new \Bic\Binary\TypedStream($stream, endianness: \Bic\Binary\Endianness::LITTLE);
$typed = new \Bic\Binary\TypedStream($stream, endianness: \Bic\Binary\Endianness::BIG);
```

### Interact With Streams

```php
/** @var \Bic\Binary\StreamInterface $stream */

$stream->read(int $bytes);      // Read X bytes
$stream->seek(int $offset);     // Seek to X offset (absolute)
$stream->move(int $offset);     // Seek to current offset + X (relative)
$stream->rewind();              // Seek to 0 offset
$stream->offset();              // Returns current offset of the stream
$stream->isCompleted();         // Returns TRUE in case of stream is completed
```

### Interact With Typed Streams

```php
/** @var \Bic\Binary\TypedStream $typed */

$typed->int8();                             // Read 1 byte as int8
$typed->byte();                             // Alias of int8()

$typed->uint8();                            // Read 1 byte as uint8
$typed->ubyte();                            // Alias of uint8()

$typed->int16();                            // Read 2 bytes as int16
$typed->short();                            // Alias of int16()

$typed->uint16([Endianness $e = null]);     // Read 2 bytes as uint16
$typed->word([Endianness $e = null]);       // Alias of uint16()
$typed->ushort([Endianness $e = null]);     // Alias of uint16()

$typed->int32();                            // Read 4 bytes as int32
$typed->int();                              // Alias of int32()
$typed->long();                             // Alias of int32()

$typed->uint32([Endianness $e = null]);     // Read 4 bytes as uint32
$typed->dword([Endianness $e = null]);      // Alias of uint32()
$typed->uint([Endianness $e = null]);       // Alias of uint32()
$typed->ulong([Endianness $e = null]);      // Alias of uint32()

$typed->int64();                            // Read 8 bytes as int64
$typed->quad();                             // Alias of int64()

$typed->uint64([Endianness $e = null]);     // Read 8 bytes as uint64
$typed->uquad([Endianness $e = null]);      // Alias of uint64()
$typed->qword([Endianness $e = null]);      // Alias of uint64()

$typed->float32([Endianness $e = null]);    // Read 4 bytes as float32
$typed->float([Endianness $e = null]);      // Alias of float32()

$typed->float64([Endianness $e = null]);    // Read 8 bytes as float64
$typed->double([Endianness $e = null]);     // Alias of float64()

// Read timestamp (DateTime object) as uint32
$typed->timestamp([Type $type = Type::UINT32], [Endianness $e = null]);

// Read data as array { int32, int32, int32, int32 }
$typed->array(4, Type::INT32);
```
