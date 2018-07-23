# mfuzz
MFuzz project

## Data format

Dataset should consist of the vectors containing the following types of values:

| Mutation                      | Write Offset   | Byte / Value ID        | Length         | Read Offset    | Seed ID | Coverage |
| ----------------------------- | -------------- | ---------------------- | -------------- | -------------- | ------- | -------- |
| `EraseBytes`                  | `0-SampleSize` | -                      | `0-SampleSize` | `0-SampleSize` | `0-N`   | `0-M`    |
| `InsertByte`                  | `0-SampleSize` | `0-255`                | `1`            | -              | `0-N`   | `0-M`    |
| `InsertRepeatedBytes`         | `0-SampleSize` | `0-255`                | `0-SampleSize` | -              | `0-N`   | `0-M`    |
| `ChangeByte`                  | `0-SampleSize` | `0-255`                | `1`            | -              | `0-N`   | `0-M`    |
| `ChangeBit`                   | `0-SampleSize` | `0-7`, (upscale)       | `1`            | -              | `0-N`   | `0-M`    |
| `ShuffleBytes *`              | `0-SampleSize` | `0-255` (rand seed)    | `0-SampleSize` | `0-SampleSize` | `0-N`   | `0-M`    |
| `CopyPart`                    | `0-SampleSize` | -                      | `0-SampleSize` | `0-SampleSize` | `0-N`   | `0-M`    |
| `AddWordFromManualDictionary` | `0-SampleSize` | `0-DictSize` (upscale) | `len(word)`    | -              | `0-N`   | `0-M`    |


Multiple mutations can be sequentially applied to the same seed input. Actually, it's encouraged to have long mutation sequences
as well as short ones. LibFuzzer's `-mutate_depth=` option can be used to increase the default value (`5`).

`SampleSize` is proposed to be restricted to `1 << 13`,  i.e. 8,192 bytes.

Number of the initial seed inputs (`N`) should be around `50`.

Coverage value (`M`) can be of any range, but we'll be normalized by the max value presented in the dataset.

\* `ShuffleBytes` function would have to be changed to use a small seed value for the sake of reproducibility. More entropy can be
gathered using hash of the testcase bytes and/or other values such as offsets and length.
