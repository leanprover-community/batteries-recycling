# HashMap

`Batteries.HashMap`, a thin wrapper around `Std.HashMap`, recycled from Batteries.

New code should use `Std.HashMap` from the Lean standard library directly. `Batteries.HashMap` has
been deprecated in its favor since 2025-04-09, and every declaration here still carries that
deprecation. This library keeps the old API available for code that still depends on it.

- `HashMap.Basic` defines `Batteries.HashMap α β`, a one-field structure over `Std.HashMap α β`,
  with `mkHashMap`, `empty`, `size`, `isEmpty`, `insert`, `insert'`, `erase`, `modify`, `find?`,
  `findD`, `find!`, `findEntry?`, `contains`, `foldM`, `fold`, `forM`, `toList`, `toArray`,
  `numBuckets`, `ofList`, `mergeWith` and `ofListWith`.

The `Std.HashMap` functions `findEntry?` and `ofListWith` that used to live in the same Batteries
file were not deprecated; they are in `Batteries.Lean.HashMap`.

```lean
import HashMap

set_option linter.deprecated false

def m : Batteries.HashMap String Nat := .ofList [("one", 1), ("two", 2)]

#eval m.find? "two"              -- some 2
#eval (m.insert "three" 3).size  -- 3
```

## Usage

Add the cell to your `lakefile.toml` and import `HashMap`:

```toml
[[require]]
name = "batteries-recycling"
git = "https://github.com/leanprover-community/batteries-recycling"
rev = "main"
```

The declarations keep their Batteries names (`Batteries.HashMap`, `Batteries.mkHashMap`), so code
that used them only needs its import changed. The library is not actively maintained.

## Acknowledgements

`HashMap` was written by Leonardo de Moura and Mario Carneiro, first in Lean core and then in Std
(now Batteries), where Mario Carneiro added most of its API and proofs in 2022. Later
contributions include:

- [batteries#1063][1] by Kim Morrison: reimplemented as a shim around `Std.HashMap`.
- [batteries#1197][2] by Markus Himmel: a one-field structure.
- [batteries#1199][3] by Kim Morrison: deprecated in favor of `Std.HashMap`.
- [batteries#2101][4]: moved to this cell.

[1]: https://github.com/leanprover-community/batteries/pull/1063
[2]: https://github.com/leanprover-community/batteries/pull/1197
[3]: https://github.com/leanprover-community/batteries/pull/1199
[4]: https://github.com/leanprover-community/batteries/pull/2101

---

Copyright (c) 2018 Microsoft Corporation. Released under the Apache 2.0 license, see
[LICENSE](../LICENSE).
