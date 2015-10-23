# -AutoLayout

Xcode运行蹦在一出，但不会有错误提示，即便错误很明显，原因未知但目测是项目太大/太旧引起的问题，把
你要调试的代码放倒新工程之中，就能看到问题提示了！


是在AutoLayout中很明显显示的一个问题，如果

       @property (weak, nonatomic)UITableView *tableView;

       #pragma mark Lazy
       - (UITableView *)tableView {
        UITableView *tableView = [[UITableView alloc] initWithFrame:CGRectMake(0, 0, 200, 300)];
        tableView.translatesAutoresizingMaskIntoConstraints = NO;
        tableView.backgroundColor = [UIColor greenColor];
        return tableView; 
        ｝




    [self.view addSubview:self.tableView];

而你在使用自动布局，如：

    // 布局
    NSDictionary *views = @{
                            @"superView" : self.view,
                            @"tableView" : self.tableView,
                            };
    NSDictionary *layoutDict = @{
                              @"V" : @"H:|-[tableView]-|",
                              @"H" : @"V:|-[tableView]-|"
                              };
    
    [NSLayoutConstraint constraintsWithVisualFormat:layoutDict[@"V"] options:0 metrics:nil views:views];
    [NSLayoutConstraint constraintsWithVisualFormat:layoutDict[@"H"] options:0 metrics:nil views:views];
    
在添加布局到self.view
会报错！提示，self.tableView未指定superView

错误原因不再AutoLayout
而是在于@property (weak, nonatomic)UITableView *tableView;的weak
然后的懒加载就会失去效果！


引以为戒！
