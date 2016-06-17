## 横向TableView    


    //首先，实例一个UITableView，并对其逆向旋转90°
    UITableView *tableview = [[UITableView alloc] init];
    [tableview setFrame:self.view.bounds];
    [self.view addSubview:tableview];
    [tableview setTransform:CGAffineTransformMakeRotation(-M_PI/2)];
    [tableview setPagingEnabled:YES];//按整页翻动
    //跳转到最后一行或者任意指定行
    NSIndexPath* indexPath = [NSIndexPath indexPathForRow:5 inSection:0];
    [tableview scrollToRowAtIndexPath:indexPath atScrollPosition:UITableViewScrollPositionMiddle animated:YES];
    
    //然后，自定义Cell，并对contentView顺时针旋转90°
    - (UITableViewCell *)tableView:(UITableView *)tableView cellForRowAtIndexPath:(NSIndexPath *)indexPath{
        static NSString *CellIdentifier = @"UITableViewCell";
        UITableViewCell *cell = [tableView dequeueReusableCellWithIdentifier:CellIdentifier];
        if (!cell) {
            cell = (UITableViewCell*)[[[NSBundle mainBundle] loadNibNamed:@"UITableViewCell" owner:nil options:nil] objectAtIndex:0];
            cell.selectionStyle = UITableViewCellSelectionStyleNone;
        }
        
        [cell.contentView setTransform:CGAffineTransformMakeRotation(M_PI/2)];
        return cell;
    }
    
