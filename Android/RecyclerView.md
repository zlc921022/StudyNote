# RecyclerView

抽取 Holder layout 绑定 放到holder 中

   public LoanTotalPaymentHolder(Context context, ViewGroup parent) {
        super(LayoutInflater.from(context).inflate(R.layout.item_loan_total_payment, parent, false));
        View view = LayoutInflater.from(context).inflate(R.layout.item_loan_total_payment, parent, false);
        ButterKnife.bind(this, view); // 这样无效 因为绑定的View 是新建的不是 holder的
    }
点击super 发现 super 传入的View 会绑定到 this.itemView = itemView; 所以 绑定itemView 就可以了

[【进阶】RecyclerView源码解析(二)——缓存机制](https://www.jianshu.com/p/e44961f8add5)</br>
[Android RecyclerView学习索引](https://www.jianshu.com/p/3726c8242aeb)</br>
[【腾讯Bugly干货分享】Android ListView 与 RecyclerView 对比浅析—缓存机制](https://zhuanlan.zhihu.com/p/23339185)</br>
[Android开发中RecyclerView的一些优化技术 mikee](http://mikeejy.github.io/2019/08/16/Android%E5%BC%80%E5%8F%91%E4%B8%ADRecyclerView%E7%9A%84%E4%B8%80%E4%BA%9B%E4%BC%98%E5%8C%96%E6%8A%80%E6%9C%AF/)</br>