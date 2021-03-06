# Python

* https://awesome-python.com/
* http://www.ianbicking.org/docs/setuptools-presentation/


## Snippets

### Miniconda installation

*   https://docs.conda.io/en/latest/miniconda.html
*   https://repo.anaconda.com/miniconda/

### Conda settings

Add conda-forge as default channel:

```sh
conda config --add channels conda-forge
conda config --set channel_priority strict
```

References:

* https://conda-forge.org/docs/

### Conda environments

Create environment file

```
conda env export > environment_droplet.yml
```

Create a conda environment from an environment file

```
conda env create -f environment.yml
```

Other commands

```
# List available conda environments
conda info --envs  

# Create environment
conda create --name [NAME]

# Remove environment and dependencies
conda remove --name [NAME] --all

# Clone existing environment
conda create --name [NEW_NAME] --clone [EXISTING_NAME]
```


### Merge .pdf files

From answer to [Merge PDF files](https://stackoverflow.com/questions/3444645/merge-pdf-files#3444735)

```python
from PyPDF2 import PdfFileMerger

pdfs = ['file1.pdf', 'file2.pdf', 'file3.pdf', 'file4.pdf']

merger = PdfFileMerger()

for pdf in pdfs:
    merger.append(pdf)

merger.write("result.pdf")
merger.close()
```

### `argparse` how-to

```python
import argparse

def isodate(s):
    fmt = '%Y-%m-%d'
    try:
        return dt.datetime.strptime(s, fmt).date()
    except ValueError:
        raise argparse.ArgumentTypeError(f'Expected `{s}` to match format `{fmt}`')

def load_args():
    parser = argparse.ArgumentParser(description='Select data.')
    parser.add_argument('--date-beg', metavar='BEG', type=isodate, required=True, help='Start date (inclusive) for the data.')
    parser.add_argument('--date-end', metavar='END', type=isodate, required=True, help='End date (inclusive) for the data.')
    return parser.parse_args()

```

### Old test approach

```python
# context.py

import os
import sys

SYS_PATH_INDEX: int = 0
parent_directory_relative = os.path.join(os.path.dirname(__file__), '..')
SYS_PATH = os.path.abspath(parent_directory_relative)
```

```python
# test_something.py


import os
import sys

import context
sys.path.insert(context.SYS_PATH_INDEX, context.SYS_PATH)

# ...
```

## Scientific Python

### Pandas

#### [How to Flatten MultiIndex Columns into a Single Index DataFrame in Pandas](https://www.pauldesalvo.com/how-to-flatten-multiindex-columns-into-a-single-index-dataframe-in-pandas/)

```python
df = pd.DataFrame(np.random.randint(3,size=(4, 3)), index = ['apples','apples','oranges','oranges'], columns=['A','B','C'])
df_grouped = df.groupby(df.index).agg({'A':['sum','mean'],'B':'sum','C':'sum'})

# One method
df_grouped.columns = ['_'.join(col) for col in df_grouped.columns.values]

# Another method
['_'.join(col) if type(col) is tuple else col for col in df.columns.values]
```

What will work depends on the datatypes in the `MultiIndex` tuples.

---

