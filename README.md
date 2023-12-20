This Jupyter notebook creates a table of COVID-19 virus lineage aliases for existing pangolin lineages.
The script uses a modified `pango_aliasor` package, which provides tools for aliasing and compressing PANGO lineages of the SARS-CoV-2 virus.
It effectively combines the following files in a manner that allows regularization of the labelling according to user preferences:
https://raw.githubusercontent.com/cov-lineages/pango-designation/master/pango_designation/alias_key.json
https://raw.githubusercontent.com/cov-lineages/pango-designation/master/lineage_notes.txt

A DataFrame is created with the compressed lineage names. The base variants are added as a new column 'alias' to the DataFrame. If a lineage does not have a base variant, the lineage name is used as the alias.
To control this the `alias_list` is a list of PANGO lineage names that an analyst wants to map to their subvariants. The aliasing is done in such a way as to partition the rows to the lowest common ancestor in the
pangolin hierarchy, i.e. it chooses the most specific alias label that applies to the row at hand.

The user supplied `pango_to_voc` dictionary maps some PANGO lineage names to their corresponding Variants of Concern (VOC) names.

Finally, rows with lineage names starting with '*' are dropped from the DataFrame, as these lineages are withdrawn. The DataFrame is then saved to a CSV file.

The `make_variant_base_map` function is defined to create a dictionary that maps each variant to its base variant. The function takes a list of base variants and a boolean flag indicating whether recombinant variants should be included. It creates an instance of the `Aliasor` class, enables expansion of the aliasor, and then partitions the base variants into their subvariants. If a base variant does not have any subvariants, it is mapped to itself.

The `replace_map` is created by calling the `make_variant_base_map` function with the `alias_list` as the argument. This map is used to replace the lineage names in the table with their base variants.

The `compressed_names` list is created by compressing the names of the internal structure in the pango_aliasor. This is a very circuitous route to get the names that are simply in lineage_notes first column.

The `pango_notes` dictionary is populated with notes for each lineage. These notes are also feteched from lineage_notes.

