from flask import Flask, redirect, render_template, request, url_for
import sqlite3

app = Flask(__name__)


def init_sqlite_db():
  conn = sqlite3.connect("database.db")
  cursor = conn.cursor()
  cursor.execute("""
        CREATE TABLE IF NOT EXISTS bugs (
            id INTEGER PRIMARY KEY AUTOINCREMENT,
            title TEXT NOT NULL,
            description TEXT NOT NULL,
            severity TEXT NOT NULL,
            status TEXT NOT NULL
        )
    """)
  conn.commit()
  conn.close()


@app.route("/")
def index():
  conn = sqlite3.connect("database.db")
  conn.row_factory = sqlite3.Row
  cursor = conn.cursor()
  cursor.execute("SELECT * FROM bugs")
  bugs = cursor.fetchall()
  conn.close()
  return render_template("index.html", bugs=bugs)


@app.route("/add", methods=["POST"])
def add_bug():
  title = request.form["title"]
  description = request.form["description"]
  severity = request.form["severity"]
  status = "Open"

  conn = sqlite3.connect("database.db")
  cursor = conn.cursor()
  cursor.execute(
      "INSERT INTO bugs (title, description, severity, status) VALUES (?, ?, ?,"
      " ?)",
      (title, description, severity, status),
  )
  conn.commit()
  conn.close()
  return redirect(url_for("index"))


@app.route("/update/<int:bug_id>", methods=["POST"])
def update_status(bug_id):
  new_status = request.form["status"]

  conn = sqlite3.connect("database.db")
  cursor = conn.cursor()
  cursor.execute(
      "UPDATE bugs SET status = ? WHERE id = ?", (new_status, bug_id)
  )
  conn.commit()
  conn.close()
  return redirect(url_for("index"))


@app.route("/delete/<int:bug_id>", methods=["POST"])
def delete_bug(bug_id):
  conn = sqlite3.connect("database.db")
  cursor = conn.cursor()
  cursor.execute("DELETE FROM bugs WHERE id = ?", (bug_id,))
  conn.commit()
  conn.close()
  return redirect(url_for("index"))


if __name__ == "__main__":
  init_sqlite_db()
  app.run(debug=True)
