# MonadSatisfying

Hoare-style postconditions for monadic programs in Lean 4, recycled from Batteries.

New code should use `Std.Do.Triple` from the Lean standard library instead. This library keeps the old API available for code that still depends on it.

- `MonadSatisfying.Basic` defines `SatisfiesM p x`, which says that the monadic value
  `x : m α` may as well have type `m {a // p a}`. It works over any functor, with
  characterizations for `Id`, `Option`, `Except`, `EStateM`, `ReaderT`, `StateT`, `StateRefT'`
  and `ExceptT`. The class `MonadSatisfying m` lifts a proof `h : SatisfiesM p x` to a value
  `satisfying h : m {a // p a}` whose underlying value is `x`, with instances for `Id`, `Option`,
  `Except`, `EStateM` and the reader, state and exception transformers.
- `MonadSatisfying.Array` and `MonadSatisfying.List` prove `SatisfiesM` results for monadic folds,
  maps and searches, such as `Array.SatisfiesM_foldlM`, `Array.SatisfiesM_mapM`,
  `Array.size_mapM` and `List.satisfiesM_foldlM`.
- `MonadSatisfying.Vector` relates `Vector.mapM`, `mapIdxM` and `mapFinIdxM` to the array versions
  through `satisfying`.
- `MonadSatisfying.Lean` is meant to hold instances for the Lean monad stack (`IO`, `CoreM`,
  `MetaM`, ...). They are commented out until they are reimplemented for the current `EIO`.

```lean
import MonadSatisfying

/-- `Array.mapM` with a proof that the size is preserved. -/
def mapMSized [Monad m] [LawfulMonad m] [MonadSatisfying m] (f : α → m β) (as : Array α) :
    m {bs : Array β // bs.size = as.size} :=
  satisfying (Array.size_mapM f as)
```

## Usage

Add the cell to your `lakefile.toml` and import `MonadSatisfying`:

```toml
[[require]]
name = "batteries-recycling"
git = "https://github.com/leanprover-community/batteries-recycling"
rev = "main"
```

The library is not actively maintained.

## Acknowledgements

`SatisfiesM` and the first monadic `Array` lemmas were written by Mario Carneiro in Std (now
Batteries) in 2022; the `Array` file credits Mario Carneiro and Gabriel Ebner. Later
contributions include:

- [batteries#696][1] by Kim Morrison: pure lemmas no longer go through `SatisfiesM`.
- [batteries#1029][2] by Kim Morrison: the `MonadSatisfying` class, `satisfying`, and instances.
- [batteries#1034][3] and [batteries#1039][4] by Kim Morrison: `List.satisfiesM_foldlM`.
- [batteries#1109][5] by Rob23oba: monadic `Array` and `Vector` proofs.
- [batteries#1172][6] by Jovan Gerbscheid: `SatisfiesM` generalized from `Applicative` to
  `Functor`.
- [batteries#1793][7] by Kim Morrison: deprecated in Batteries and moved to recycling.
- [batteries#1868][8] by François G. Dorais: moved to `Batteries.Recycling`.
- [batteries#1955][9]: moved to this cell.

[1]: https://github.com/leanprover-community/batteries/pull/696
[2]: https://github.com/leanprover-community/batteries/pull/1029
[3]: https://github.com/leanprover-community/batteries/pull/1034
[4]: https://github.com/leanprover-community/batteries/pull/1039
[5]: https://github.com/leanprover-community/batteries/pull/1109
[6]: https://github.com/leanprover-community/batteries/pull/1172
[7]: https://github.com/leanprover-community/batteries/pull/1793
[8]: https://github.com/leanprover-community/batteries/pull/1868
[9]: https://github.com/leanprover-community/batteries/pull/1955

---

Copyright (c) 2021 Mario Carneiro and 2024 Lean FRO, LLC. Released under the Apache 2.0 license,
see [LICENSE](../LICENSE).
