### UIScrollView Paging for any size data
> Use sample from apple

1. Get `totalCount` of all records from server, then load 10 records once a time.
2. Set uiscrollView's contentSize width `self.scrollView.frame.size.width * totalCount`
3. Get current dataSource `dataSource` and `tappedIndex` when user entered.
4. Load `tappedIndex and tappedIndex + 1` 's data into scrollview, and set `ContentOffset` to be `tappedIndex * pageWidht + 1`;


### Issue: load data from server

when scroll end, check index and load if possible