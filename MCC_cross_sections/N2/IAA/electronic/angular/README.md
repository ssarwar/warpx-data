# N2 electronic angular distributions

Pass a `.angular` descriptor as `differential_cross_section`. Each descriptor
references its normalized `_pdf.txt` profile and the shared elastic fallback.
The profile contains incident energy followed by values in sr⁻¹ at 0°, 1°, …,
180°. It supplies only angular probability; the integral collision rate and
energy loss come from the parent directory and gas manifest.

[sources.json](sources.json) records measured rows, canonical mappings, endpoint
completion, transitions and source hashes.

| Descriptor | Profile |
|---|---|
| [b_1Pi_u.angular](b_1Pi_u.angular) | [b_1Pi_u_pdf.txt](b_1Pi_u_pdf.txt) |
| [C_3Pi_u.angular](C_3Pi_u.angular) | [C_3Pi_u_pdf.txt](C_3Pi_u_pdf.txt) |
| [a_1Pi_g.angular](a_1Pi_g.angular) | [a_1Pi_g_pdf.txt](a_1Pi_g_pdf.txt) |
| [c4_prime_1Sigma_u_plus.angular](c4_prime_1Sigma_u_plus.angular) | [c4_prime_1Sigma_u_plus_pdf.txt](c4_prime_1Sigma_u_plus_pdf.txt) |
| [b_prime_1Sigma_u_plus.angular](b_prime_1Sigma_u_plus.angular) | [b_prime_1Sigma_u_plus_pdf.txt](b_prime_1Sigma_u_plus_pdf.txt) |
| [B_3Pi_g.angular](B_3Pi_g.angular) | [B_3Pi_g_pdf.txt](B_3Pi_g_pdf.txt) |
| [c3_1Pi_u.angular](c3_1Pi_u.angular) | [c3_1Pi_u_pdf.txt](c3_1Pi_u_pdf.txt) |
| [W_3Delta_u.angular](W_3Delta_u.angular) | [W_3Delta_u_pdf.txt](W_3Delta_u_pdf.txt) |
| [o3_1Pi_u.angular](o3_1Pi_u.angular) | [o3_1Pi_u_pdf.txt](o3_1Pi_u_pdf.txt) |
| [A_3Sigma_u_plus.angular](A_3Sigma_u_plus.angular) | [A_3Sigma_u_plus_pdf.txt](A_3Sigma_u_plus_pdf.txt) |
| [B_prime_3Sigma_u_minus.angular](B_prime_3Sigma_u_minus.angular) | [B_prime_3Sigma_u_minus_pdf.txt](B_prime_3Sigma_u_minus_pdf.txt) |
| [a_double_prime_1Sigma_g_plus.angular](a_double_prime_1Sigma_g_plus.angular) | [a_double_prime_1Sigma_g_plus_pdf.txt](a_double_prime_1Sigma_g_plus_pdf.txt) |
| [a_prime_1Sigma_u_minus.angular](a_prime_1Sigma_u_minus.angular) | [a_prime_1Sigma_u_minus_pdf.txt](a_prime_1Sigma_u_minus_pdf.txt) |
| [w_1Delta_u.angular](w_1Delta_u.angular) | [w_1Delta_u_pdf.txt](w_1Delta_u_pdf.txt) |
| [E_3Sigma_g_plus.angular](E_3Sigma_g_plus.angular) | [E_3Sigma_g_plus_pdf.txt](E_3Sigma_g_plus_pdf.txt) |
| [G_3Pi_u.angular](G_3Pi_u.angular) | [G_3Pi_u_pdf.txt](G_3Pi_u_pdf.txt) |
| [F_3Pi_u.angular](F_3Pi_u.angular) | [F_3Pi_u_pdf.txt](F_3Pi_u_pdf.txt) |
