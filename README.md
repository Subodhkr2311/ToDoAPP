MainActivity
package com.example.todoapp;



import android.app.Dialog;
import android.content.Context;
import android.util.Log;
import android.view.LayoutInflater;
import android.view.View;
import android.view.ViewGroup;
import android.view.animation.AnimationUtils;
import android.widget.Button;
import android.widget.EditText;
import android.widget.Toast;

import androidx.annotation.NonNull;
import androidx.recyclerview.widget.RecyclerView;

import java.util.ArrayList;

public class adapterRecycle extends RecyclerView.Adapter<TaskViewHolder> {
    ArrayList<xyz> arr;
    Context c;

    public adapterRecycle(Context c, ArrayList<xyz> arr) {
        this.arr = arr;
        this.c = c;
    }

    @NonNull
    @Override
    public TaskViewHolder onCreateViewHolder(@NonNull ViewGroup parent, int viewType) {
        View v = LayoutInflater.from(c).inflate(R.layout.tasklayout, parent, false);
        return new TaskViewHolder(v);
    }

    @Override
    public void onBindViewHolder(@NonNull TaskViewHolder holder, int position) {
        holder.taskDescription.setText(arr.get(position).taskD);
        holder.taskDueDate.setText(arr.get(position).taskDueD);
        holder.taskCompletionStatus.setChecked(arr.get(position).taskCompletion);
        holder.taskCompletionStatus.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                if(holder.taskCompletionStatus.isChecked()){
                    holder.card.setBackgroundColor(c.getResources().getColor(R.color.green));
                } else{
                    holder.card.setBackgroundColor(c.getResources().getColor(R.color.defaultcc));
                }

                }

        });
        holder.card.setOnLongClickListener(new View.OnLongClickListener() {
            @Override
            public boolean onLongClick(View v) {
                Dialog dd= new Dialog(c);
                dd.setContentView(R.layout.deletell);
                Button b1= dd.findViewById(R.id.DeleteDD);
                Button b2= dd.findViewById(R.id.cancelDD);
                b1.setOnClickListener(new View.OnClickListener() {
                    @Override
                    public void onClick(View v) {
                        arr.remove(position);
                        notifyItemRemoved(position);
                        notifyItemRangeChanged(position, arr.size());
                        dd.dismiss();

                    }
                });
                b2.setOnClickListener(new View.OnClickListener() {
                    @Override
                    public void onClick(View v) {
                        dd.dismiss();
                    }
                });
                dd.show();
                return true;
            }
        });

        holder.card.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                Dialog db= new Dialog(c);
                db.setContentView(R.layout.addtodo);
                EditText e1= db.findViewById(R.id.edAddtask);
                EditText e2= db.findViewById(R.id.taskDueDateEditText);
                Button b1= db.findViewById(R.id.materialButton);
                Button b2= db.findViewById(R.id.materialButton2);
                b2.setOnClickListener(new View.OnClickListener() {
                    @Override
                    public void onClick(View v) {
                        db.dismiss();
                    }});
                b1.setText("Update");
                b1.setOnClickListener(new View.OnClickListener() {
                    @Override
                    public void onClick(View v) {
                        String td, tdd;
                        td= e1.getText().toString();
                        tdd= e2.getText().toString();
                        if(td.isEmpty() && tdd.isEmpty()){
                            Toast.makeText(c, "Add task and due date", Toast.LENGTH_SHORT).show();
                        }else {
                            arr.set(position, new xyz(td, tdd, false));

                            notifyItemChanged(arr.size()-1);
                            db.dismiss();
                        }
                    }
                });

 db.show();
            }
        });
    }

    @Override
    public int getItemCount() {
        return arr.size();
    }
}

