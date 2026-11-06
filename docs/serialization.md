# Weight serialization format

**Status:** not decided. Settle this before running experiments 03 and 04, and don't change it mid-experiment.

Decide and record:

- Numeric precision (decimal places or significant figures) and notation
- Layout: one tensor per block, row-major, header naming the layer and shape
- Separators and line breaks
- Whether biases sit with their weights or in their own block
- Any preamble given to the reader model

Changing any of these is its own experimental condition (see 04, print precision).
