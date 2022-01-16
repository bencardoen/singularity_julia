# README
Contains a template to create a [Singularity](https://sylabs.io/guides/3.5/user-guide/) Julia container with 1.7.1 on Fedora 35, with a user specified package pre-installed. 

Use case : you're sharing private or public work but don't want your users to have to install Julia, deal with dependencies etc.

## Usage
```bash
sudo singularity build myimage.sif singularity_template.def
```
After creation:
```bash
singularity exec myimage.sif julia --project=/opt/MyPackage.jl
```

```bash
singularity shell myimage.sif
Singularity>julia --project=/opt/MyPackage.jl
```

## Notes
- This sets the JULIA_DEPOT_PATH variable (inside the container) to a container folder, to ensure we're using the environment **inside** the container, not outside. 
If you prefer to use the outside Julia env, remove the depot path steps, but note this will write to ~/.julia.
- Julia interactive use will write to *logs*, in order to enable this without making the container writable, we map the log directory to /dev/shm (tmpfs on Linux), which by POSIX should exist and be user writable. For execution of scripts you do not need this, so you could omit it.

## Customizing
Replace MyPackage.jl with your own package. Optionally add extra dependencies (dnf or conda, ...).
