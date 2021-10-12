# Netflow

[Netflow](https://en.wikipedia.org/wiki/NetFlow)

[Type of Service](https://en.wikipedia.org/wiki/Type_of_service), or ToS

# Code analysis

Get a group from GroupBy object:
```
firg = gb.get_group(list(gb.groups)[0])
secg = gb.get_group(list(gb.groups)[1])
```

Get how many rows in each group: `gb.size()`.
However, `gb.count()` print each column count in each group.
For *NaN* is not count by `count()` and `Sport` in second group (whose `SrcAddr` is 00:15:17:2c:e5:2d)
is NaN, so you can see it's 0 in the output of `gb.count()`.

See [Pandas Groupby – Count of rows in each group](https://datascienceparichay.com/article/pandas-groupby-count-of-rows-in-each-group)
for detailed comparison between `size()` and `count()`.

The whole process of *Random forest* algorithms is:

1. For raw input dataset (.binetflow file), *Window_lower* and *Window_upper_excl* is calculated based on *StartTime* column, *window_width * and *window_stride*;
1. The whole dataset is devided into multiple gruops according to their *Window_lower* and *Window_upper_excl* (`gb = data.loc[...` in the `for` loop in preprocessing1);
1. For each group, statistical is calculated and are saved to `X`;
1. `Label_<lambda>` (statistics of `Label`) is the response variable;
1. There're totally 7 unique values in the "Label" column, where `flow=From-Botne` is `Yes` (from botnet),
   while other 6 values are `No`, which saved in the variable `y_bin6`;
1. X and y (`y_bin6`) and split into train and test set: `model_selection.train_test_split()`: 
1. A new training set (X_train_new, y_train_new) are created through resampling to 20 times of original size: `utils.resample()`;
1. The new training set is fit to the `ensemble.RandomForestClassifier`: `clf.fit(X_train_new, y_train_new)`;
1. The model is evaluated on training and test set: `clf.predict(X_train_new)` and `clf.predict(X_test)`;
