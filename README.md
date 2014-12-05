## Read jpeg images in Go, quickly

This library is the fastest possible way to decode and encode JPEG images in Go.

We achieve this via cgo bindings to [libjpeg-turbo](http://libjpeg-turbo.virtualgl.org)
library.

The exact speed depends on the image and CPU. On Mac Book Pro, compared
to `image/jpeg` standard library, golibjpegturbo is:
* 6x faster when decoding
* 1.7x faster when encoding at quality level of 90%

You can rerun the benchmark on your machine with `go test -bench=.`

Additionally, unlike `image/jpeg`, the library can read jpeg images in CMYK format.

## Setup

Before you import library, you need to install libjpeg-turbo.

On Ubuntu: `sudo apt-get install libjpeg-turbo8-dev`.

On Mac OS X: `apt-get install libjpeg-turbo`

## Usage

`go get github.com/kjk/golibjpegturbo`

The API is the same as `image/libjpeg`:

```go
import "golibjpegturbo"

func decode(r io.Reader) (image.Image, error) {
    return golibjpegturbo.Decode(r)
}

func encode(img image.Image) ([]byte, error) {
    options := &golibjpegturbo.Options{Quality: 90}
    var buf bytes.Buffer
    err = golibjpegturbo.Encode(&buf, decodedImg, options)
    if err != nil {
        return nil, err
    }
    return buf.Bytes(), nil
}

```

## License

MIT.

Written by [Krzysztof Kowalczyk](http://blog.kowalczyk.info/).

