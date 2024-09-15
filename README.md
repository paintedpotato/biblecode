# biblecode
Wanna find codes in the Bible?

#### Result:
There are <b>21</b> mentions of TVRH (Torah) in the Hebrew Bible, in sequences of 50
but there are <b>25</b> for YHWH

## Documentation for the Letter Indexing Code with Hebrew Words

---

### **Purpose:**
The code is designed to process a CSV file containing verses from the Torah (in Hebrew), split each verse into its component words, extract individual letters from each word, and assign an index to every letter. The final output is saved as a CSV file where each letter is paired with its index and the Hebrew word from which it was extracted.

---

### **Steps of the Program:**

1. **Importing Required Libraries:**
   The `pandas` library is imported to handle the CSV file and manipulate the data efficiently.
   ```python
   import pandas as pd
   ```

2. **Loading the CSV File:**
   The CSV file (`cleaned_merged_file_sefaria_full.csv`) containing Hebrew verses is loaded into a pandas DataFrame. Each row in the DataFrame represents one verse.
   ```python
   df = pd.read_csv('cleaned_merged_file_sefaria_full.csv', header=None)
   ```

3. **Initialization:**
   We initialize an empty list called `letters` to store the indexed letters along with their corresponding Hebrew words. We also start an `index` variable at 1, which will sequentially number the letters.
   ```python
   letters = []
   index = 1
   ```

4. **Processing Each Verse:**
   The program iterates over each verse in the DataFrame:
   - The verse text is first cleaned by removing spaces using `replace(" ", "")` to ensure only meaningful characters (letters) are processed.
   - The verse is split into its constituent words using `split()`, and then each word is processed individually.
   ```python
   for _, row in df.iterrows():
       verse = row[0].replace(" ", "")  # Clean the verse by removing spaces
       words = row[0].split()           # Split the verse into words
   ```

5. **Processing Each Word:**
   Each word is processed by cleaning any residual spaces and then extracting each letter within that word.
   - The `cleaned_word` variable stores the current word after removing spaces.
   - The letters from this word are then appended to the `letters` list along with their index and the cleaned word to which they belong.
   ```python
   for word in words:
       cleaned_word = word.replace(" ", "")  # Clean any spaces in the word
       
       for letter in cleaned_word:
           letters.append([index, letter, cleaned_word])  # Store the index, letter, and word
           index += 1  # Increment the letter index
   ```

6. **Storing Results in a DataFrame:**
   Once all the letters and their associated data (index and word) are processed, the information is stored in a new pandas DataFrame with three columns: `Index`, `Letter`, and `Hebrew Word`.
   ```python
   letters_df = pd.DataFrame(letters, columns=['Index', 'Letter', 'Hebrew Word'])
   ```

7. **Saving the Data to a New CSV File:**
   The final DataFrame is saved as a CSV file (`indexed_letters_with_words.csv`) using `to_csv()`. This ensures that the indexed letters and words are written to disk for further use.
   ```python
   letters_df.to_csv('indexed_letters_with_words.csv', index=False)
   ```

8. **Output Message:**
   A confirmation message is printed indicating that the CSV file has been successfully created.
   ```python
   print("Indexed letters and their respective words have been saved successfully to 'indexed_letters_with_words.csv'.")
   ```

---

### **Output:**
The resulting CSV file (`indexed_letters_with_words.csv`) contains the following structure:
- **Index**: The sequential index of the letter (starting from 1).
- **Letter**: The Hebrew letter extracted from the text.
- **Hebrew Word**: The word from which the letter was extracted (in Hebrew).

Example output in the CSV file:
```
Index,Letter,Hebrew Word
1,ב,בראשית
2,ר,בראשית
3,א,בראשית
4,ש,בראשית
5,י,בראשית
6,ת,בראשית
7,ב,ברא
...
```

---

### **Usage:**
This script is particularly useful for tasks such as:
- **Textual analysis**: Analyzing the structure and distribution of letters in Hebrew scripture.
- **Gematria or Letter Analysis**: For applications involving Hebrew letters, numerology, or the study of letter sequences in sacred texts.
- **Further research**: This format allows easy indexing and querying of specific letters or words for research purposes.

---

### **Requirements:**
- **Python 3.x**
- **Pandas Library** (install with `pip install pandas`)

Make sure the file `cleaned_merged_file_sefaria_full.csv` is structured properly with one verse per row in the Hebrew language before running this script.
