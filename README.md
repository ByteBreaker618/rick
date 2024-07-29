# rick
rock
import json
import os

class Movie:
    def __init__(self, title, director, year):
        self.title = title
        self.director = director
        self.year = year

    def __str__(self):
        return f"'{self.title}' directed by {self.director} ({self.year})"

class MovieCollection:
    def __init__(self, filename="movies.json"):
        self.filename = filename
        self.movies = []
        self.load_movies()

    def load_movies(self):
        if os.path.exists(self.filename):
            with open(self.filename, "r") as file:
                movies_data = json.load(file)
                self.movies = [Movie(**data) for data in movies_data]
        else:
            self.movies = []

    def save_movies(self):
        with open(self.filename, "w") as file:
            json.dump([movie.__dict__ for movie in self.movies], file, indent=4)

    def add_movie(self, title, director, year):
        new_movie = Movie(title, director, year)
        self.movies.append(new_movie)
        self.save_movies()

    def remove_movie(self, title):
        self.movies = [movie for movie in self.movies if movie.title != title]
        self.save_movies()

    def edit_movie(self, old_title, new_title, new_director, new_year):
        for movie in self.movies:
            if movie.title == old_title:
                movie.title = new_title
                movie.director = new_director
                movie.year = new_year
                self.save_movies()
                break

    def display_movies(self):
        if not self.movies:
            print("No movies found.")
        else:
            for movie in self.movies:
                print(movie)
                print("-" * 30)

def main():
    collection = MovieCollection()

    while True:
        print("\nMovie Collection Menu")
        print("1. Add movie")
        print("2. Remove movie")
        print("3. Edit movie")
        print("4. View movies")
        print("5. Exit")

        choice = input("Enter your choice: ")

        if choice == "1":
            title = input("Enter movie title: ")
            director = input("Enter movie director: ")
            year = input("Enter movie year: ")
            collection.add_movie(title, director, year)
        elif choice == "2":
            title = input("Enter the title of the movie to remove: ")
            collection.remove_movie(title)
        elif choice == "3":
            old_title = input("Enter the title of the movie to edit: ")
            new_title = input("Enter new movie title: ")
            new_director = input("Enter new movie director: ")
            new_year = input("Enter new movie year: ")
            collection.edit_movie(old_title, new_title, new_director, new_year)
        elif choice == "4":
            collection.display_movies()
        elif choice == "5":
            break
        else:
            print("Invalid choice. Please try again.")

if __name__ == "__main__":
    main()
