# clone-lin-aave-wadray

Experimental LIN clone of Aave V3 `WadRayMath.wadMul` / `wadDiv` (i64-positive scale: 128-bit `(a*b+HALF_WAD)/WAD` half-up, fail-closed if the quotient does not fit i64). `RAY=1e27` is unrepresentable in u64 (`awr_ray_fits=0`).

- **Upstream:** [aave/aave-v3-core](https://github.com/aave/aave-v3-core) `contracts/protocol/libraries/math/WadRayMath.sol` commit `782f51917056a53a2c228701058a6c3fb233684a` (MIT after BUSL-1.1 Change Date 2023-01-27).
- **LIN kernel:** [`src/lin_aave_wadray.lin`](src/lin_aave_wadray.lin)
- **C11 oracle copy:** [`test/oracles/aave_wadray_c11.c`](test/oracles/aave_wadray_c11.c)
- **Proofs (lin-open only):** [kbelludoo/lin-open](https://github.com/kbelludoo/lin-open) `examples/aave_wadray/` and `python3 test/prove_aave_wadray_external.py`
- **Class:** EXPERIMENTAL. Not Ethereum Aave V3 uint256. Not RAY 1e27. Not a production replacement. Not Solady/OZ floor `mulWad`.

Named goldens (i64-positive, WAD=1e18): `wadMul(1e18,1e18)` → **1000000000000000000**; `wadMul(3, HALF_WAD)` → **2** vs floor **1**; `wadDiv(2,3)` → **666666666666666667** vs floor **666666666666666666**; `wadMul(3e18,3e18)` → **9000000000000000000** vs naive i64 wrap **5595889181738926080**; `wadMul(4e18,3e18)` fail-closed **0**; `awr_ray_fits` → **0**; `wadToRay(1e18)` fail-closed **0** vs naive wrap **-6930898827444486144**.
