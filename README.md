# A1R — 1-Methyl-3-Phenyl-7-Benzylamino-Pyrazolo[3,4-d]pyridazine Antagonists

Molecular dynamics (MD) characterization of a pyrazolo[3,4-d]pyridazine antagonist
series at the human adenosine A1 receptor (A1R), including a cross-receptor
comparison against the adenosine A3 receptor (A3R) to probe binding selectivity.

## Repository contents

| Folder | Description |
|---|---|
| `starting_structures/` | The docking-pose complex (receptor + ligand, pre-simulation) used to seed each MD system. |
| `representative_frames/` | One representative post-equilibration frame extracted directly from each system's production trajectory. |

Each file is named `<system>_complex.pdb` or `<system>_run<N>_frame<F>.pdb`, where
`<system>` identifies the receptor/ligand pairing (see table below) and, for the
MD frames, `run<N>`/`frame<F>` record exactly which replica and trajectory frame
the structure was extracted from.

## Systems simulated

| System | Receptor | Ligand | Replicas | Length per replica |
|---|---|---|---|---|
| A1-PPZ1 | A1R | PPZ1 | 2 | 500 ns |
| A1-10 | A1R | Compound 10 | 2 | 500 ns |
| A1-11 | A1R | Compound 11 | 2 | 500 ns |
| A1-12 | A1R | Compound 12 | 2 | 500 ns |
| A1-16 | A1R | Compound 16 | 2 | 500 ns |
| A3-11 | A3R | Compound 11 | 2 | 500 ns |

All receptors are embedded in an explicit lipid bilayer (POPC/POPE, Amber `lipid21`)
with TIP3P water, aligned using OPM prior to system assembly.

## Methods summary

- **System preparation:** receptor + membrane assembled with `packmol-memgen`/OPM
  alignment; ligand docked into the orthosteric pocket.
- **Force fields:** protein — `ff19SB`; lipids — `lipid21`; water — `TIP3P`;
  ligand — GAFF2 with AM1-BCC partial charges (via `antechamber`).
- **Equilibration:** staged minimization/heating/density-equilibration protocol
  (AmberMdPrep), followed by production.
- **Production MD:** AMBER24 `pmemd.cuda`, hydrogen mass repartitioning (HMR),
  4 fs timestep, semi-isotropic NPT, 500 ns per independent replica.
- **Analysis:**
  - RMSD (protein Cα and ligand heavy atoms) via `cpptraj`/MDAnalysis.
  - Protein–ligand interaction fingerprints via [ProLIF](https://prolif.readthedocs.io/),
    averaged across replicas with SEM, including water-mediated (bridge)
    interactions.
  - Residues labeled with GPCRdb generic (Ballesteros–Weinstein) numbering for
    cross-receptor comparison.
