# dictionarydef dictionary_app():
    # Pre-populated word dictionary
    word_dict = {
        "python": "A high-level, interpreted programming language.",
        "variable": "A reserved memory location to store values.",
        "algorithm": "A set of rules to be followed in calculations or problem-solving operations."
    }

    while True:
        print("\n=== PYTHON DICTIONARY MENU ===")
        print("1. Search a word")
        print("2. Add a new word")
        print("3. View all words")
        print("4. Exit")
        
        choice = input("Enter choice (1-4): ").strip()

        if choice == "1":
            word = input("Enter word to search: ").strip().lower()
            meaning = word_dict.get(word, "Word not found in dictionary.")
            print(f"\nMeaning: {meaning}")

        elif choice == "2":
            word = input("Enter new word: ").strip().lower()
            meaning = input("Enter meaning: ").strip()
            word_dict[word] = meaning
            print(f"'{word}' has been added successfully!")

        elif choice == "3":
            print("\n--- Dictionary Contents ---")
            for key, val in word_dict.items():
                print(f"• {key.title()}: {val}")

        elif choice == "4":
            print("Exiting dictionary app. Goodbye!")
            break
        else:
            print("Invalid choice. Please choose between 1 and 4.")

if __name__ == "__main__":
    dictionary_app()
