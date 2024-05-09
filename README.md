RBE Scorer extensions for OpenTOPAS
---

  - David Hall. First commit 07/2017.
  - Jan Schuemann. Major revision 11/2019.
  - Jose Ramos-Mendez. Update to OpenTOPAS v4.0/Geant4 v11.1.3 05/2024
    
---

## Installation.

> [!NOTE]
> The main difference in installing for Debian/Ubuntu and MacOS is the installation paths. 

> [!TIP]
> Users can install multiple extensions by either:
> - Copy all the Extension directories into a common global directory, or
> - Using the `\;` between directory paths e.g., (and see below):
>
>      `-DTOPAS_EXTENSIONS_DIR=/MyProject1Extensions/\;/MyProject2Extensions/`

### Debian

Follow [installation for Debian/Ubuntu](https://opentopas.readthedocs.io/en/latest/getting-started/Debian.html) procedures after and including **step 8.4**. Then:

    $ cd $HOME/Applications/

Clone the repository:

    $ git clone https://github.com/OpenTOPAS/OpenTOPAS-RBE.git

Return to the topas-build directory:

    $ cd $HOME/Applications/TOPAS/OpenTOPAS-build

Repeat `cmake` command by including the `TOPAS_EXTENSIONS_DIR` variable as follows (don't miss the `-D`):

    $ cmake ../OpenTOPAS -DCMAKE_INSTALL_PREFIX=../OpenTOPAS-install -DTOPAS_EXTENSIONS_DIR=$HOME/Applications/OpenTOPAS-RBE

Build:

    $ make -j8 install

### MacOS

Follow [installation for MacOS](https://opentopas.readthedocs.io/en/latest/getting-started/MacOS.html) procedures after and including **step 7.4**. Then:

    $ cd /Applications

Clone the repository:

    $ git clone https://github.com/OpenTOPAS/OpenTOPAS-RBE.git

Return to the topas-build directory:

    $ cd /Applications/TOPAS/OpenTOPAS-build

Repeat `cmake` command by including the `TOPAS_EXTENSIONS_DIR` variable as follows (don't miss the `-D`):

    $ cmake ../OpenTOPAS -DCMAKE_INSTALL_PREFIX=../OpenTOPAS-install -DTOPAS_EXTENSIONS_DIR=/Applications/OpenTOPAS-RBE

Build:

    $ make -j8 install
    
---

## Implementation of the models and corresponding references can be found at:  

### :books: Polster, et al., "Extension of TOPAS for the simulation of proton radiation effects considering molecular and cellular endopoints" [doi:10.1088/0031-9155/60/13/5053](doi:10.1088/0031-9155/60/13/5053)

## Full documentation is found at

### :books: [https://OpenTOPAS.readthedocs.io/en/latest](https://OpenTOPAS.readthedocs.io/en/latest/)

---

These extension scorers require OpenTOPAS version 4.0 or later.

The directory consists of the following file types:
Scorers:

   . TsScoreDose* : These score quantities in the ProcessHits function like normal scorers.

RBE Scorers: These do not have a ProcessHits function and instead combine scored
properties (dose, LET, etc) to RBE or biological dose, etc.

   . TsV*: Base classes for the scorers

Additionally, the example directory contains an example experiment irradiation
(experiment.txt) scoring each of the available RBE scorers (rbe_scorers.txt) for
V79 cells (`CellLineV79.txt`). V79 cells are used because they are one of the most
studied cells and biological parameters for all models were available.
The simulations can be run with "topas run.txt" and analyzed with the provided
python script.

In order to change the experimental setup edit `experiment.txt`_.

In order to change the cell line, provide a new cell line file and change

   sv:Sc/CellLines     = 1 "CellLineV79"

in run.txt.

`run.txt` also controls the Prescribed dose used to calculate RBE and the output
quantity. The output quantities available depend on the RBE model.

RBE scorers are defined in rbe_scorers.txt and can be edited there. Typically,
we recommend not to run too many scorers at once as that increases memory use.
In particular, the two parameters:

    `ReferencedSubScorer_Dose`

and

    `ReferencedSubScorer_LET`

should be set if a dose and LET scorer already exists, otherwise each RBE scorer
will create scorers for all properties it needs, potentially resulting in
multiple parallel dose scorers.
