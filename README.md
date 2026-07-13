## Booth multiplier

✅ Parameterizable Booth encoder
✅ Parameterizable partial product generator
✅ Parameterizable radix-4 Booth multiplier
✅ Exhaustively verified against the built-in multiplier
✅ Synthesized to the GF180 standard-cell library
✅ Measured ASIC area (~7433 µm²)


![alt text](https://media.discordapp.net/attachments/1507718965168701583/1525821184694943804/image.png?ex%3D6a54c736%26is%3D6a5375b6%26hm%3D7b93534890beb355410ded30f91ad58465e503dcc5906b234224492fe457fbe7%26%3D%26format%3Dwebp%26quality%3Dlossless%26width%3D1934%26height%3D710)


Performed exhaustive testing on 256x256 cases on a self testing bench and compared against a*b (generic) and has passed in all the 67564 cases.



## Area Comparison of 8-bit Multiplier Architectures (GF180 MCU PDK)

| Version | Multiplier Architecture | MAC Cell Area (µm²) | Improvement vs Generic | Remarks |
|:--------:|-------------------------|--------------------:|-----------------------:|---------|
| V1 | Generic `a*b` Multiplier | **15,221.52** | Baseline | Synthesized using inferred multiplier |
| V2 | Radix-4 Booth Multiplier (Initial) | **12,679** | **16.7%** | Encoder + Partial Product Generator |
| V3 | Radix-4 Booth Multiplier (Optimized) | **12,100** | **20.5%** | Optimized correction logic and sign extension |
| V4 | Booth + Carry Save Adder (CSA) Reduction | **12,594** | **17.3%** | CSA overhead outweighed benefits for an 8×8 multiplier |

### Summary

- **Baseline MAC Area:** 15,221.52 µm²
- **Best Achieved Area:** 12,100 µm²
- **Total Area Reduction:** 3,121.52 µm²
- **Overall Improvement:** **20.5%**

### Observation

Although a Carry Save Adder (CSA) tree is beneficial for multipliers with a large number of partial products, it did **not** reduce the area for an 8×8 Radix-4 Booth multiplier. Since Radix-4 Booth encoding already reduces the multiplication to only **four partial products**, the synthesis tool (Yosys + ABC) generated a more area-efficient implementation using direct addition than the manually constructed CSA tree. Consequently, the optimized Booth multiplier without CSA was selected as the final architecture.



| Architecture                | Status                   | Reason                                                       |
| --------------------------- | ------------------------ | ------------------------------------------------------------ |
| Generic inferred multiplier | ✓ Implemented            | Baseline reference                                           |
| Radix-4 Booth multiplier    | ✓ Selected               | Lowest synthesized area                                      |
| Booth + CSA reduction       | ✗ Evaluated but rejected | Higher area than direct reduction for 8×8 operands           |
| Wallace-tree reduction      | Planned / Future Work    | Expected to benefit larger operand widths (16-bit and above) |
