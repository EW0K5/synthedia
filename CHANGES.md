## v1.0.3
- Reduced memory footprint
- Speed improvements
- Added ```--select_n``` argument to simulate a random subset of input peptides
- Fixed bug with ```--write_empty_spectra``` flag

## v1.0.2
- Added single-peptide command-line preview functions to quickly test parameter sets
- Added additional metadata to mzml scan headers
- Added additional input parameters to give fine control of the distribution of chromatographic peak widths

## v1.0.1
- Added ```--write_params``` argument
- Added ```--output_label``` prefix to logging and pickle files
- Fixed crash in `peptide.get_limits` when the ms_clip_window produced an empty mask; now logs a warning and returns an empty slice instead of exiting. Added guards to calculate_feature_windows to avoid zero-width windows and additional debug logging in mzml.py.  
- Added safeguard in `mzml.Spectrum.add_profile_peaks` to skip peaks when the computed intensity array is empty, avoiding `max()` on an empty sequence and logging a warning.

## v1.0.0
- First public release of synthedia
