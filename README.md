# Nadav Final Project

in my lab we have countred a problem were we need to design [PCR primers](https://www.youtube.com/watch?v=NODrmBHHni8&ab_channel=Henrik%27sLab) but the tools on the market these days are expensive so the planning is done manually and inefficiently.

when planned manualy you need to select the sequence yourself and then analyze it using various online tools to get GC content, secondary structure and [Primer dimer](https://kilobaser.com/the-pain-of-primer-dimer/) which will all have effect on the PCR reaction efficiency.

I want to build a tool that will get a sequence and produce primers for that sequence.

In order to do so I am using these libraries and modules:

- [Biopython](https://biopython.org/)
- [ViennaRNA Package 2](https://www.tbi.univie.ac.at/RNA/documentation.html#)
- other standard packges like numpy and pandas

The application can be used to design primers for any DNA sequence, providing a comprehensive solution for primer design and analysis. The graphical user interface simplifies the process of entering sequence data and viewing the results, enhancing the user experience and productivity. The tool is designed to be intuitive and user-friendly, making it accessible to researchers and professionals involved in molecular biology and genetics.

The application is designed to be modular and extensible, allowing for the addition of new features and functionalities in the future. It can be integrated with other tools and platforms to enhance its capabilities and provide a more comprehensive solution for primer design and analysis. The codebase is well-documented and structured, making it easy to understand and maintain, and the application can be further optimized for performance and scalability.

for further information and explanation please refer to the [documentation](documentation.md)

**Step 1**: Install all required packages whcih include: [Biopython](https://biopython.org/), [ViennaRNA Package 2](https://www.tbi.univie.ac.at/RNA/documentation.html#), [tkinter](https://docs.python.org/3/library/tkinter.html), [pandas](https://pandas.pydata.org/pandas-docs/stable/getting_started/install.html).

```python
pip install biopython
pip install viennarna
pip install tkinter
pip install pandas
```

**Step 2**: download python files: Main.py, Primerdesigner.py, GUI.py and place them in the same directory.

**Step 3**: Run the Main.py file using the command line or an IDE. After running the file the next window will open up:
<div align="center">
    <img src="Pictures/start_menu.png" alt="Start Menu" width="200">
</div>
When pressing the drop down menu two options will show raw text and uploading a fasta file:
<div align="center">
    <img src="Pictures/start_dropdown.png" alt="Dropdown Menu" width="200">
</div>
selsect on of the options.

**Step 4**: Enter the DNA sequence or upload a FASTA file to begin the analysis.

When "raw text" is selected the next window will appear, enter a valid DNA sequence in the text box. a valid DNA sequence consists of A, T, G, C and the program is **not** case sensitive.
<div align="center">
    <img src="Pictures/textInput.png" alt="Raw Text Input" width="400">
</div>
When entering an invalid input you wont be able to submit your sequence and it will alert you that the sequence is invalid
<div align="center">
    <img src="Pictures/invalid_textInput.png" alt="Invalid Text Input" width="400">
</div>
If the "FASTA file" option is selected the next window will apear:
<div align="center">
    <img src="Pictures/FASTA_input.png" alt="FASTA input" width="400">
</div>
when pressing the "Browse" button a file brower will open up allowing the selection of .FASTA files only. 
after selecting a file a "Submit" button will appear.

pressing the "Submit" will send the user to the next window.


**Step 5**: Customize primer design parameters 

The next window will apear with several prameter that are devided into two sections:

The first section is "Amplicon Placement" in this section the user is asked to define what will be amplified by the primers there are 3 options to select from:
- Length: will take the first nuckeutide in the submitted to be the first in the amplified sequence and the user can set how long will it be meaning the value entred is the end nucleutide. The default is the sequence length.
- Range: Is similar to the length but here the first nucleutide is defined by the user.
- All: this option will take the entire sequnce and define it a the amplified region.

The second section is "Other Parameters" in this section the primer parameters are defined, the default values appear in the inacitve spin boxes and the user can change them if needed by ticking the box next to the parameter. 

The parameters are:
- Primer Length: the length of the primer, the default is 20.
- GC Content: the GC content of the primer, the default is 40%.
- Primer Tm: the melting temperature of the primer, the default is 55.
- ∆G: the free energy of the primer, the default is -10.

<div align="center">
    <img src="Pictures/para_input.png" alt="Parameters" width="400">
</div>

when done press the "Submit" button and move to the next window.

**Step 6**: View primer design results

The next window will show the results of the primer design, including the forward and reverse primers, their sequences, and the primer properties. The user can view the results and save them to a file for future reference.

<div align="center">
    <img src="Pictures/output_menu.png" alt="Results" width="600">
</div>










