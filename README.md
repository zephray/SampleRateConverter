# Sample Rate Converter

This is a C implementation of an audio sample rate convertor based on Polyphase FIR filter. It can be used to up or downconverting the sample rate of a raw audio stream. The stream should be de-interleaved in 32bit float format. 

## Usage

Use as a normal half-band FIR filter:

```C
#include <src.h>
int num_taps = 24;
float cutoff_freq = 0.5;
float* coefficients = src_generate_fir_coeffs(num_taps, cutoff_freq);
FIR_Filter* filter = src_generate_fir_filter(h, num_taps, 1, 1);
...
float* input_buffer[IN_SIZE]; // Incoming stream
float* output_buffer[IN_SIZE]; // Outcoming stream
src_filt(filter, input_buffer, input_length, output_buffer);
```

Convert from 44.1kHz to 48kHz:

```C
#include <src.h>
int interpolation = 160;
int decimation = 147;
int num_taps = 24 * interpolation;
float* coefficients = src_generate_fir_coeffs(num_taps, 0.5 / interpolation);
FIR_Filter* filter = src_generate_fir_filter(h, num_taps, interpolation, decimation);
...
float* input_buffer[IN_SIZE]; // Incoming stream
float* output_buffer[IN_SIZE * interpolation / decimation + 1]; // Outcoming stream
int output_length = src_filt(filter, input_buffer, input_length, output_buffer);
```

## License

MIT.

Julia-DSP library source code was used as a reference, which is also licensed under MIT.