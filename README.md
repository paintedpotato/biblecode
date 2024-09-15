# biblecode
Wanna find codes in the Bible?

#### Result:
There are <b>21</b> mentions of TVRH (Torah) in the Hebrew Bible, in sequences of 50
but there are <b>25</b> for YHWH

### Documentation for the Letter Indexing Code with Hebrew Words and Pattern Search

---

### **Purpose:**
The script processes a CSV file containing Hebrew verses from the Torah, extracts individual letters from each word, and assigns an index to every letter. Additionally, the code searches for specific letter sequences—**Tav (ת), Vav (ו), Resh (ר), and Hey (ה)**—which are spaced exactly **50 letters apart**. These sequences are useful in various textual analyses, such as detecting patterns or codes within the Hebrew text.

---

### **Steps of the Program:**

#### 1. **Importing Required Libraries:**
The `pandas` library is used to handle CSV file manipulation, and basic Python functionality is used for indexing and searching specific patterns.

```python
import pandas as pd
```

#### 2. **Loading the CSV File:**
We load the CSV file (`cleaned_merged_file_sefaria_full.csv`) containing the Hebrew verses into a pandas DataFrame. Each row in the DataFrame represents one verse.

```python
df = pd.read_csv('cleaned_merged_file_sefaria_full.csv', header=None)
```

#### 3. **Initialization:**
- An empty list `letters` is created to store each indexed letter and the word it belongs to.
- A variable `index` is initialized to 1, which sequentially numbers each letter in the Torah.

```python
letters = []
index = 1
```

#### 4. **Processing Each Verse:**
- The script iterates over every verse.
- Spaces are removed from the verse using `replace(" ", "")` to ensure that only meaningful letters are processed.
- The cleaned verse is split into its individual words for further processing.

```python
for _, row in df.iterrows():
    verse = row[0].replace(" ", "")  # Remove spaces from the verse
    words = row[0].split()  # Split the verse into individual words
```

#### 5. **Processing Each Word:**
- For each word, any remaining spaces are cleaned.
- Each letter in the word is indexed and appended to the list, associating the letter with its respective word.

```python
for word in words:
    cleaned_word = word.replace(" ", "")
    
    for letter in cleaned_word:
        letters.append([index, letter, cleaned_word])  # Store the index, letter, and word
        index += 1
```

#### 6. **Finding Specific Letter Patterns (Tav, Vav, Resh, Hey):**
- After indexing all letters, we search for the specified sequence **Tav (ת), Vav (ו), Resh (ר), Hey (ה)**, where each letter is spaced exactly 50 characters apart.
- A `for` loop iterates over the indexed letters list, checking for the required pattern:
  - If a **Tav (ת)** is found, it checks the next letters at +50, +100, and +150 positions to match **Vav (ו), Resh (ר), and Hey (ה)**.
  - The index positions of valid sequences are saved.

```python
# Find sequences of Tav, Vav, Resh, Hey separated by 50 letters
pattern_indices = []

for i in range(len(letters) - 150):
    if letters[i][1] == 'ת' and letters[i+50][1] == 'ו' and letters[i+100][1] == 'ר' and letters[i+150][1] == 'ה':
        pattern_indices.append([letters[i][0], letters[i+50][0], letters[i+100][0], letters[i+150][0]])  # Store indices of valid sequences
```

#### 7. **Storing Results in a DataFrame:**
- The indexed letters, along with their corresponding Hebrew word, are saved in a new DataFrame. This file includes the indexed letters and any found sequences for the **Tav, Vav, Resh, Hey** pattern.

```python
letters_df = pd.DataFrame(letters, columns=['Index', 'Letter', 'Hebrew Word'])
letters_df.to_csv('indexed_letters_with_words.csv', index=False)
```

#### 8. **Output:**
- Two CSV files are generated:
  - **`indexed_letters_with_words.csv`**: This contains every letter indexed along with the word it belongs to.
  - **`pattern_indices.csv`**: This contains the index positions where the sequence **Tav, Vav, Resh, Hey** occurs, spaced by 50 letters.

```python
pattern_df = pd.DataFrame(pattern_indices, columns=['Tav_Index', 'Vav_Index', 'Resh_Index', 'Hey_Index'])
pattern_df.to_csv('pattern_indices.csv', index=False)
```

#### 9. **Completion Message:**
Once the files are saved, the script prints a message indicating that the letters have been successfully indexed and that the pattern search results have been saved.

```python
print("Indexed letters and patterns have been saved successfully.")
```

---

### **Files Generated:**
1. **`indexed_letters_with_words.csv`**:
   - Columns: `Index`, `Letter`, `Hebrew Word`
   - Each row represents a letter with its index and the word it belongs to.
   
2. **`pattern_indices.csv`**:
   - Columns: `Tav_Index`, `Vav_Index`, `Resh_Index`, `Hey_Index`
   - This file contains the index positions of the **Tav, Vav, Resh, Hey** sequences found at 50-letter intervals.

---

### **Usage:**
This code can be useful for:
- **Textual Analysis**: Analyzing letter sequences and their distributions within the Hebrew Torah.
- **Gematria or Torah Code Research**: Finding patterns of letters spaced by fixed intervals in sacred texts.
- **Scholarly or Educational Use**: For understanding Hebrew letter patterns and their occurrences within the Torah.

---

### **Requirements:**
- **Python 3.x**
- **Pandas Library** (Install with `pip install pandas`)

Make sure to provide the `cleaned_merged_file_sefaria_full.csv` file, where each row contains a single verse in Hebrew before running the script.

---
### Trivia:
#### 0. **Data Collection & Cleanup:**

##### **Manual Download and Programmatic Merging of the First Five Books of the Torah from Sefaria**

##### **A. Manual Download from Sefaria**
To work with the full Hebrew text of the Torah, we manually downloaded the first five books (Genesis, Exodus, Leviticus, Numbers, and Deuteronomy) from **Sefaria.org**, a comprehensive digital library for Jewish texts. This process involved:

##### i. **Accessing Each Book**: 
   - Visiting the Torah section on Sefaria for each of the five books.
   - Downloading the text in **Hebrew** for Genesis (Bereshit), Exodus (Shemot), Leviticus (Vayikra), Numbers (Bamidbar), and Deuteronomy (Devarim).
   
###### ii. **Saving the Texts**: 
   - Each book was saved as a separate file in **CSV format** (or any plain text format), ensuring the Hebrew text for each verse was preserved. The text was stored in rows, with each row containing a single verse from the corresponding book.

---

##### **B. Programmatic Merging of the Files**
Once the individual books were downloaded, we merged them into a single file programmatically using **Python**. Here’s how:


###### i. **Loading the Individual Files**: 
   - Each downloaded CSV file (representing a book) was loaded into a **pandas DataFrame**. This allowed us to handle each file’s data efficiently in memory.

   ```python
   import pandas as pd
   
   # Load each file into a pandas DataFrame
   genesis_df = pd.read_csv('genesis.csv')
   exodus_df = pd.read_csv('exodus.csv')
   leviticus_df = pd.read_csv('leviticus.csv')
   numbers_df = pd.read_csv('numbers.csv')
   deuteronomy_df = pd.read_csv('deuteronomy.csv')
   ```


###### ii. **Merging the Data**: 
   - We concatenated the DataFrames from the five books into one single DataFrame, ensuring that the Hebrew text was kept in order from the first verse of Genesis to the last verse of Deuteronomy.

   ```python
   # Merge all the books into one DataFrame
   merged_df = pd.concat([genesis_df, exodus_df, leviticus_df, numbers_df, deuteronomy_df], ignore_index=True)
   ```


###### iii. **Saving the Merged Data**: 
   - The combined DataFrame was then saved as a new CSV file (`merged_file_sefaria_full.csv`), which contained all the verses from the Torah in one file.

   ```python
   # Save the merged file
   merged_df.to_csv('merged_file_sefaria_full.csv', index=False)
   ```

---

##### **C. Manual Cleanup of the Merged File**
After merging, we noticed the file had various formatting issues (such as extra spaces and misaligned text). The following manual steps were taken to clean up the file:


###### i. **Removing Unnecessary Spaces**: 
   - We carefully reviewed the Hebrew text to remove extra spaces between letters and words.
   

###### ii. **Ensuring Consistent Format**: 
   - Each row in the merged file was manually reviewed to ensure that the Hebrew verses were properly aligned. This included removing any extraneous information (like footnotes or metadata) that might have been included during the download.


###### iii. **Final Adjustments**:
   - After manually cleaning the file, we ensured that every verse was in its respective row, ready for further processing.

The cleaned file was then saved as `cleaned_merged_file_sefaria_full.csv`.

---

##### **Summary of the Process**
- We used **Sefaria** to download the Hebrew text of the Torah's first five books manually.
- These books were merged programmatically using **pandas** in Python.
- Manual steps were taken to clean the merged file, ensuring the Hebrew text was in an indexable format.
- The cleaned and formatted CSV file was used as the input for further text analysis, including indexing letters and searching for specific patterns within the Torah.

The final result was a **clean and structured CSV file** that could be further manipulated and indexed for any analytical purposes.



This updated documentation includes both the letter indexing and the pattern search functionality for **Tav, Vav, Resh, and Hey** separated by 50 letters.
