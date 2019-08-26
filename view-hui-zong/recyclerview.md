# RecyclerView

### 基本使用

{% code-tabs %}
{% code-tabs-item title="RecycleView的Adapter" %}
```java

class StudentAdapter extends RecyclerView.Adapter<StudentAdapter.MyViewHolder> {

    private List<Student> mStudentList;

    public StudentAdapter(List<Student> studentList) {
        mStudentList = studentList;
    }

    @NonNull
    @Override
    public MyViewHolder onCreateViewHolder(@NonNull ViewGroup parent, int viewType) {
        View view = LayoutInflater.from(parent.getContext()).inflate(R.layout.fruit_item, parent, false);
        MyViewHolder holder = new MyViewHolder(view);
        return holder;
    }

    @Override
    public void onBindViewHolder(@NonNull MyViewHolder holder, int position) {
        Student student = mStudentList.get(position);
        holder.mIv.setImageResource(R.mipmap.ic_launcher);
        holder.mTv.setText("学生：" + student.toString());
    }

    @Override
    public int getItemCount() {
        return mStudentList.size();
    }

    public class MyViewHolder extends RecyclerView.ViewHolder {

        private final ImageView mIv;
        private final TextView mTv;

        public MyViewHolder(@NonNull View itemView) {
            super(itemView);
            mIv = itemView.findViewById(R.id.iv_rv_item);
            mTv = itemView.findViewById(R.id.tv_rv_item);
        }
    }
}
```
{% endcode-tabs-item %}
{% endcode-tabs %}



{% code-tabs %}
{% code-tabs-item title="整体使用" %}
```java
        RecyclerView rv = findViewById(R.id.rv_test);
        List<Student> list = new ArrayList<>();
        list.add(new Student("张三", 18));
        list.add(new Student("李四", 18));
        list.add(new Student("王二", 18));
        list.add(new Student("赵武", 18));
        list.add(new Student("杨三", 18));
        list.add(new Student("郭靖", 18));


        StudentAdapter adapter = new StudentAdapter(list);
        rv.setAdapter(adapter);


        LinearLayoutManager manager = new LinearLayoutManager(this);
        rv.setLayoutManager(manager);
```
{% endcode-tabs-item %}
{% endcode-tabs %}

