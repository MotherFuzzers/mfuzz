# mfuzz
MFuzz project

## Data format

Dataset should consist of the vectors containing the following types of values:

| Mutation                      | Write Offset   | Byte / Value ID        | Length         | Read Offset    |
| ----------------------------- | -------------- | ---------------------- | -------------- | -------------- |
| `EraseBytes`                  | `0-SampleSize` | -                      | `0-SampleSize` | `0-SampleSize` |
| `InsertByte`                  | `0-SampleSize` | `0-255`                | `1`            | -              |
| `InsertRepeatedBytes`         | `0-SampleSize` | `0-255`                | `0-SampleSize` | -              |
| `ChangeByte`                  | `0-SampleSize` | `0-255`                | `1`            | -              |
| `ChangeBit`                   | `0-SampleSize` | `0-7`, (upscale)       | `1`            | -              |
| `ShuffleBytes *`              | `0-SampleSize` | `0-255` (rand seed)    | `0-SampleSize` | `0-SampleSize` |
| `CopyPart`                    | `0-SampleSize` | -                      | `0-SampleSize` | `0-SampleSize` |
| `AddWordFromManualDictionary` | `0-SampleSize` | `0-DictSize` (upscale) | `len(word)`    | -              |


Multiple mutations can be sequentially applied to the same seed input. Actually, it's encouraged to have long mutation sequences
as well as short ones. LibFuzzer's `-mutate_depth=` option can be used to increase the default value (`5`).

`SampleSize` is proposed to be restricted to `1 << 13`,  i.e. 8,192 bytes.

Number of the initial seed inputs (`N`) should be around `50`.

Coverage value (`M`) can be of any range, but we'll be normalized by the max value presented in the dataset.

\* `ShuffleBytes` function would have to be changed to use a small seed value for the sake of reproducibility. More entropy can be
gathered using hash of the testcase bytes and/or other values such as offsets and length.
