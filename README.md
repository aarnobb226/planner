# plannerdaily-planner/  
├── app.py  
├── templates/  
│   └── planner.html  
└── static/  
    └── styles.css
    from flask import Flask, render_template, request, session, redirect, url_for  
import secrets  
  
app = Flask(__name__)  
app.secret_key = secrets.token_hex(16)  # Random secure key  
  
@app.route('/', methods=['GET', 'POST'])  
def planner():  
    if 'tasks' not in session:  
        session['tasks'] = [{'text': '', 'done': False} for _ in range(30)]  
      
    if request.method == 'POST':  
        task_index = int(request.form.get('task_index'))  
        session['tasks'][task_index]['text'] = request.form.get('task_text', '')  
        session['tasks'][task_index]['done'] = not session['tasks'][task_index]['done']  
        session.modified = True  
        return redirect(url_for('planner'))  
      
    return render_template('planner.html', tasks=session['tasks'])  
  
if __name__ == '__main__':  
    app.run(debug=True)
