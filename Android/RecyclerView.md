# RecyclerView

抽取 Holder layout 绑定 放到holder 中

   public LoanTotalPaymentHolder(Context context, ViewGroup parent) {
        super(LayoutInflater.from(context).inflate(R.layout.item_loan_total_payment, parent, false));
        View view = LayoutInflater.from(context).inflate(R.layout.item_loan_total_payment, parent, false);
        ButterKnife.bind(this, view); // 这样无效 因为绑定的View 是新建的不是 holder的
    }
点击super 发现 super 传入的View 会绑定到 this.itemView = itemView; 所以 绑定itemView 就可以了

[【进阶】RecyclerView源码解析(二)——缓存机制](https://www.jianshu.com/p/e44961f8add5)

[Android RecyclerView学习索引](https://www.jianshu.com/p/3726c8242aeb)

[【腾讯Bugly干货分享】Android ListView 与 RecyclerView 对比浅析—缓存机制](https://zhuanlan.zhihu.com/p/23339185)