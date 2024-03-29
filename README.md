from flask import Flask, request, jsonify

app = Flask(__name__)

# Sample data - tasks stored in memory
tasks = [
    {"id": 1, "title": "how to basic", "completed": False},
    {"id": 2, "title": "how to drink", "completed": False}
]

# Endpoint to get all tasks
@app.route('/tasks', methods=['GET'])
def get_tasks():
    return jsonify(tasks)

# Endpoint to create a new task
@app.route('/tasks', methods=['POST'])
def create_task():
    data = request.json
    new_task = {
        "id": len(tasks) + 1,
        "title": data["title"],
        "completed": False
    }
    tasks.append(new_task)
    return jsonify(new_task), 201

# Endpoint to update a task
@app.route('/tasks/<int:task_id>', methods=['PUT'])
def update_task(task_id):
    for task in tasks:
        if task['id'] == task_id:
            task['title'] = request.json.get('title', task['title'])
            task['completed'] = request.json.get('completed', task['completed'])
            return jsonify(task)
    return jsonify({"error": "Task not found"}), 404

# Endpoint to delete a task
@app.route('/tasks/<int:task_id>', methods=['DELETE'])
def delete_task(task_id):
    for task in tasks:
        if task['id'] == task_id:
            tasks.remove(task)
            return jsonify({"message": "Task deleted successfully"})
    return jsonify({"error": "Task not found"}), 404

if __name__ == '__main__':
    app.run(debug=True)
