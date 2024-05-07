# word-generator

This code is designed to analyze and manipulate numerical word formations based on certain rules and constraints. It provides functionalities to process numerical levels, generate word formations, exclude specific words, and export results to CSV format.

## Dependencies
- `re`: Regular expression module for pattern matching.
- `copy`: Module for creating deep copies of objects.

## Usage

### `LevelTracker`
This class is responsible for tracking the occurrences of each number from a level.

#### Methods

##### `__init__(self, depth, content)`

Track the occurrence of every number from a level.

Attributes:
- `depth`: The depth of the numerical level we are tracking.
- `content`: The list of numbers in this level.

##### `track(self)`

Start tracking the occurrence of each number from this level.

Returns:
- A dictionary whose key is a number and value is the number of times it appears in this level.

### `NumericalLevels`
Represents a data structure that stores numerical values of each level from a text file. It provides methods to populate and track levels.

#### Methods

##### `__init__(self, filename, generators)`

Data structure that stores the numerical values of each level from the text file.

Attributes:
- `filename`: The name of the .txt file that stores all the numerical levels.
- `generators`: The list of Apery numbers of a given numerical semigroup.

##### `populate(self)`

Reads in the data and stores them in two formats as two new attributes of the object.

Returns:
- `self.original_levels`: An array storing the numerical values in each level allowing duplicacy.
- `self.clumped_levels`: An array storing the numerical values in each level without any duplicated number.

##### `tracking(self)`

Assigns each level a dictionary that stores the number of occurrences for each element in said level.

### `NumericalWord`
Stores a numerical value and all its valid word formations. It includes methods to add word formations and modify them.

#### Methods

##### `__init__(self, numerical_value)`

Data structure that stores a numerical value and all of its valid word formations.

Attributes:
- `numerical_value`: A type-int object that stores the numerical value of an element in a numerical level.
- `words`: An array that stores all possible word formations of a number.

##### `__str__(self)`

Return str(self).

##### `addWord(self, string)`

Add a possible word formation to the numerical value.

##### `modify(self, letter)`

Find a match from the next level and then add the corresponding letter to the end of all word formations we have so far for this numerical value.


### `WordGenerator`
This class is the highest-level structure, storing information about numerical values from all levels of a numerical semigroup. It includes functionalities to satisfy constraints, execute word formations, prettify, nitpick, exclude specific words, and export to CSV format.
    
#### Attributes

- `regex`: A regular expression that represents the most general form of the language.
- `generators`: The list of Apery numbers of a given numerical semigroup.
- `dictionary`: A dictionary to converse an Apery number to its corresponding letter in the alphabet.
- `reverse_dictionary`: The key-value pair inversion of `self.dictionary`.

#### Methods

##### `__init__(self, regex, generators)`
Initializes the `WordGenerator` object.

Params:
- `regex`: A regular expression representing the language.
- `generators`: A list of Apery numbers.

##### `exclude(self, wordsList, excluded_words=[''], mode=0, verbose='quiet', freq=0)`

See the cascading effect of excluding a list of specific words of some numerical values from a level.

Params:
- `wordsList`: The 2D lists of words that need to be checked.
- `excluded_words`: The array of words that need to be excluded.
- `mode`: Functioning mode (default 0).
- `verbose`: Set to something else if you want to print out the new list after excluding unwanted word.
- `freq`: Reserved specifically for mode 3.

Returns:
- `modified_words_list`: The new word list from `wordsList` after excluding unwanted words.

##### `execute(self, filename)`

Builds all the word formations of each numerical value in every level.

Returns:
- `self.words_list`: A new attribute of `WordGenerator` object that stores all possible words formation from all levels as a 2D array.

##### `nitpick(self, wordsList, mode=1, freq=None, excess=None)`

Print out any number that has the same/different number of words as the number of times it occurs.

Params:
- `wordsList`: The 2D lists of words that need to be checked.
- `mode`: The functioning mode.
- `freq`: Reserved specifically for mode 3.
- `excess`: Reserved specifically for mode 2.

Returns:
- `excedance`: A flatten-out 1D array that stores the amount of excess each word has, reserved specifically for mode 0.

##### `prettify(self, wordsList)`

Print out the content of a words list.

Params:
- `wordsList`: The 2D lists of words that need to be printed out.

##### `satisfy(self, string)`

Prototype for the set of rules that might be implemented in a single class in the future. For now, we're only concerned with the general form of the language.

Params:
- `string`: A possible word formation that needs to be checked with `self.regex` and `self.startwith`.

Returns:
- `True`: If the word formation fits in with `self.regex`.
- `False`: If otherwise.

##### `to_csv(self, target_list, filename)`

Export a given words list to a separate .csv file.

Params:
- `target_list`: The words list that we want to export.
- `filename`: The name for the .csv file.

## How to Use

1. **Initialization**:
   - Create a `WordGenerator` object by providing a regular expression and a list of generators.
   - Optionally set `startwith` attribute.

2. **Execution**:
   - Call the `execute()` method with the filename containing numerical levels.

3. **Analysis**:
   - Utilize methods like `prettify()` to print word formations, `nitpick()` to print specific words based on conditions, and `exclude()` to exclude unwanted words.

4. **Export**:
   - Export the word formations to a CSV file using `to_csv()`.

## Example

```python
# Initialize WordGenerator object
gen = WordGenerator(
    regex = r'(a(b|c|d|e)*)|((b|c|d|e)*)',
    generators = [5, 91, 162, 253, 304],
)
# Execute word formations
generator.execute(filename='layers.txt')

# Print word formations
generator.prettify(generator.words_list)

# Exclude unwanted words
first_iteration  = gen.exclude(
    gen.words_list,
    excluded_words = ['cb'],
    mode = 1
)
# Export to CSV
generator.to_csv(first_iteration, filename='word_formations.csv')
