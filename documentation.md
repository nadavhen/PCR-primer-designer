# Primer Design Application Documentation

## Overview
This documentation covers the main components of a primer design application, including `PrimerDesigner.py`, `Main.py`, and `GUI.py`. The application provides a comprehensive solution for designing primers, integrating sequence input, parameter customization, and result presentation through a user-friendly graphical interface.

The application accepts a DNA sequence and operates in two modes:
- Raw text: the user can enter a DNA sequence directly into the text box.
- FASTA file: the user can upload a FASTA file containing a DNA sequence.
Furthermore the user can customize the primer design parameters such as:
- Primer length
- GC content
- Melting temperature
- Free energy

The application then displays the results of the primer design, including the forward and reverse primers, their sequences, and the primer properties. The user can view the results and save them to a file for future reference.
the primers are designed using a sliding window approach, which is then analyzed for the following parameters:
- GC content
- Melting temperature
- Free energy
- Secondary structure
- Binding sites

filters are then applied to the primers based on the parameters set by the user and the results are displayed in a table format. if no primers are found in the first run the application will try to find primers with a larger length and if no primers are found the application will alert the user that no primers were found.

## Main.py

### Overview
Serves as the entry point, integrating GUI components with primer design logic.

### Workflow
1. Initializes the GUI for sequence input.
2. Collects sequence and parameters for primer design.
3. Utilizes `PrimerDesigner` for primer generation and filtering.
4. Displays results for user review and saving.

### Dependencies
- Biopython, RNA library for `PrimerDesigner.py`.
- `tkinter` and `pandas` for `GUI.py`.

## PrimerDesigner.py

### Overview
A Python module for designing primers for a given DNA sequence using Biopython and the RNA library.

### Key Functions
- `primer_design`: Generates forward and reverse primers.
- `ListToPrimes`: Converts sequences to Primer objects.
- `checkBindingSites`: Counts primer binding sites in a sequence.
- `filterPrimers`: Filters primers based on various criteria.

### Class `Primer`
Represents a primer with properties like sequence, GC content, and melting temperature.

### Dependencies
- `Biopython`
- `RNA`
## GUI.py

### Overview
Creates graphical user interfaces for the application using `tkinter`.

### Key Functions
- `run_gui`: Runs the main GUI for sequence input.
- `para_gui`: GUI for inputting primer design parameters.
- `displayAndSave_gui`: Displays results and provides a save option.

### Dependencies
- `tkinter`
- `pandas`

This documentation encapsulates the modular design and functionality of a primer design application, offering insights into its components and usage.

# Detailed Function Documentation

## PrimerDesigner.py Functions

### `primer_design(sequence: str, start: int, end: int, p_length: int = 20) -> Tuple[List[Seq], List[Seq]]`
- **Inputs**:
  - `sequence` (str): The DNA sequence for which primers are to be designed.
  - `start` (int): The starting position in the sequence for primer design.
  - `end` (int): The ending position in the sequence for primer design.
  - `p_length` (int, optional): The desired length of the primers. Defaults to 20.
- **Outputs**: A tuple containing two lists: the list of forward primers and the list of reverse primers.
- **Description**: Generates forward and reverse primers for a given DNA sequence based on specified start and end positions and primer length.
- **Example Usage**:
    ```python
    sequence = "AGCTTAGCTAGCTTACGATCGATCGTACGATCGTACGATCGATCG"
    start = 10
    end = 30
    p_length = 20

    forward_primers, reverse_primers = primer_design(sequence, start, end, p_length)

    print("Forward Primers:", forward_primers)
    print("Reverse Primers:", reverse_primers)
    ```

### `ListToPrimes(prime_list: list) -> List[Primer]`
- **Inputs**:
  - `prime_list` (list): A list of sequences to be converted into Primer objects.
- **Outputs**: A list of `Primer` objects.
- **Description**: Converts a list of primer sequences into Primer objects for object-oriented manipulation and analysis.
- **Example Usage**:
    ```python
    primer_sequences = ['AGCTTAGCTA', 'CGTACGATCG']
    primer_objects = ListToPrimes(primer_sequences)

    for primer in primer_objects:
        print(primer)
    ```

### `checkBindingSites(primer_df, sequence: str) -> pd.DataFrame`
- **Inputs**:
  - `primer_df`: A DataFrame containing primer sequences.
  - `sequence` (str): The DNA sequence to check for primer binding sites.
- **Outputs**: A DataFrame with an additional column showing the count of binding sites for each primer.
- **Description**: Evaluates the DNA sequence for the number of potential binding sites for each primer, assisting in assessing primer specificity.
- **Example Usage**:
    ```python
    import pandas as pd

    # Assuming primer_df is a DataFrame containing a column 'Sequence' with primer sequences
    primer_df = pd.DataFrame({'Sequence': ['AGCTTAGCTA', 'CGTACGATCG']})
    sequence = "AGCTTAGCTAGCTTACGATCGATCGTACGATCGTACGATCGATCG"

    primer_df_with_binding_sites = checkBindingSites(primer_df, sequence)

    print(primer_df_with_binding_sites)
    ```

### `filterPrimers(primer_df, max_binding_sites: int, max_tm: float, min_tm: float, min_dg: float, min_gc: float, max_gc: float) -> pd.DataFrame`
- **Inputs**:
  - Various parameters for filtering primers, such as the maximum number of binding sites, melting temperature range, free energy, and GC content range.
- **Outputs**: A filtered DataFrame of primers.
- **Description**: Filters primers based on specified criteria to ensure they meet the design requirements.
- **Example Usage**:
    ```python
    max_binding_sites = 3
    max_tm = 60.0
    min_tm = 50.0
    min_dg = -20.0
    min_gc = 40.0
    max_gc = 60.0

    filtered_primers = filterPrimers(primer_df_with_binding_sites, max_binding_sites, max_tm, min_tm, min_dg, min_gc, max_gc)

    print(filtered_primers)
    ```

## Class `Primer`
The `Primer` class represents a primer with its sequence and various properties calculated based on the sequence. It encapsulates functionality for calculating properties such as GC content, melting temperature (Tm), secondary structure, and free energy (ΔG), which are essential for assessing primer suitability in PCR reactions.

### Attributes
- `sequence` (str): The nucleotide sequence of the primer.
- `length` (int): The length of the primer sequence.
- `gc_content` (float): The GC content percentage of the primer sequence.
- `tm` (float): The melting temperature of the primer, calculated using the nearest-neighbor method.
- `secondary_structure` (str): The predicted secondary structure of the primer sequence.
- `dg` (float): The free energy associated with the secondary structure, indicative of primer stability.

### Methods

#### `__init__(self, sequence: str)`
Constructor method to initialize a Primer object with its sequence and calculate its properties.
- **Inputs**:
  - `sequence` (str): The nucleotide sequence of the primer.
- **Actions**: Calculates the primer's length, GC content, melting temperature, secondary structure, and free energy upon initialization.

#### `__str__(self)`
Returns a string representation of the Primer object, including its sequence and calculated properties.
- **Outputs**: A string detailing the primer's sequence, length, GC content, melting temperature, secondary structure, and ΔG.

#### `to_dict(self)`
Converts the Primer object's attributes into a dictionary, facilitating easier integration with data handling libraries like pandas.
- **Outputs**: A dictionary containing the primer's properties, including its sequence, length, GC content, melting temperature, secondary structure, and ΔG.

### Example Usage
```python
primer_sequence = "ATCGAGCTAA"
primer = Primer(primer_sequence)
print(primer)
# Outputs a string representation of the primer and its properties

primer_properties = primer.to_dict()
print(primer_properties)
```


## GUI.py Functions

### `run_gui() -> Tuple[str, str]`
- **Outputs**: A tuple indicating the input type ('file' or 'text') and the corresponding DNA sequence or file path.
- **Description**: Initiates the GUI for sequence input, allowing users to input a sequence directly or select a file, with input validation.

### `para_gui(length_default: int) -> dict`
- **Inputs**:
  - `length_default` (int): Default length for validating DNA sequence inputs.
- **Outputs**: A dictionary of user-selected parameters for primer design, such as primer length, GC content, melting temperature, and free energy.
- **Description**: Presents a GUI for inputting detailed primer design parameters, with input validation against predefined constraints.

### `displayAndSave_gui(df: pd.DataFrame)`
- **Inputs**:
  - `df` (pd.DataFrame): The DataFrame containing the primer design results.
- **Description**: Shows the primer design results in a table format within the GUI, offering an option to save the results to a file.

