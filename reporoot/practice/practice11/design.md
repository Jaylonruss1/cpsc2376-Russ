#include <iostream>
#include <vector>
#include <string>
#include <algorithm>

class Game {
public:
    // Constructor to initialize game with name and total copies
    Game(std::string name, int totalCopies)
        : name(name), totalCopies(totalCopies), availableCopies(totalCopies) {}

    // Getter for the game name
    std::string getName() const {
        return name;
    }

    // Getter for the number of available copies
    int getAvailableCopies() const {
        return availableCopies;
    }

    // Method to borrow a copy of the game
    void borrowCopy() {
        if (availableCopies > 0) {
            --availableCopies;
        } else {
            std::cout << "No copies available to borrow!" << std::endl;
        }
    }

    // Method to return a copy of the game
    void returnCopy() {
        if (availableCopies < totalCopies) {
            ++availableCopies;
        } else {
            std::cout << "All copies are already available!" << std::endl;
        }
    }

private:
    std::string name;
    int totalCopies;
    int availableCopies;
};

class Customer {
public:
    // Constructor to initialize the customer with their name
    Customer(std::string name)
        : name(name) {}

    // Getter for the customer name
    std::string getName() const {
        return name;
    }

    // Method to borrow a game
    void borrowGame(std::string gameName) {
        borrowedGames.push_back(gameName);
    }

    // Method to return a borrowed game
    void returnGame(std::string gameName) {
        auto it = std::find(borrowedGames.begin(), borrowedGames.end(), gameName);
        if (it != borrowedGames.end()) {
            borrowedGames.erase(it);
        } else {
            std::cout << "You haven't borrowed this game!" << std::endl;
        }
    }

    // Display all the borrowed games by the customer
    void viewBorrowedGames() const {
        if (borrowedGames.empty()) {
            std::cout << name << " has not borrowed any games." << std::endl;
        } else {
            std::cout << name << " has borrowed the following games:" << std::endl;
            for (const auto& game : borrowedGames) {
                std::cout << game << std::endl;
            }
        }
    }

private:
    std::string name;
    std::vector<std::string> borrowedGames;
};

class LendingSystem {
public:
    // Method to add a game to the inventory
    void addGame(std::string name, int totalCopies) {
        games.push_back(Game(name, totalCopies));
    }

    // Method to check out a game to a customer
    void checkOutGame(std::string gameName, std::string customerName) {
        auto gameIt = std::find_if(games.begin(), games.end(),
            [&](const Game& g) { return g.getName() == gameName; });
        auto customerIt = std::find_if(customers.begin(), customers.end(),
            [&](const Customer& c) { return c.getName() == customerName; });

        if (gameIt != games.end() && customerIt != customers.end()) {
            gameIt->borrowCopy();
            customerIt->borrowGame(gameName);
        } else {
            std::cout << "Game or Customer not found!" << std::endl;
        }
    }

    // Method to return a borrowed game
    void returnGame(std::string gameName, std::string customerName) {
        auto gameIt = std::find_if(games.begin(), games.end(),
            [&](const Game& g) { return g.getName() == gameName; });
        auto customerIt = std::find_if(customers.begin(), customers.end(),
            [&](const Customer& c) { return c.getName() == customerName; });

        if (gameIt != games.end() && customerIt != customers.end()) {
            gameIt->returnCopy();
            customerIt->returnGame(gameName);
        } else {
            std::cout << "Game or Customer not found!" << std::endl;
        }
    }

    // Method to view all available games in the inventory
    void viewInventory() const {
        std::cout << "Game Inventory:" << std::endl;
        for (const auto& game : games) {
            std::cout << game.getName() << " - Available: " << game.getAvailableCopies() << std::endl;
        }
    }

    // Method to view all borrowed games
    void viewBorrowedGames() const {
        std::cout << "Currently Borrowed Games:" << std::endl;
        for (const auto& customer : customers) {
            customer.viewBorrowedGames();
        }
    }

    // Method to register a new customer
    void addCustomer(std::string name) {
        customers.push_back(Customer(name));
    }

private:
    std::vector<Game> games;
    std::vector<Customer> customers;
};

int main() {
    LendingSystem lendingSystem;

    // Adding games to the system
    lendingSystem.addGame("Monopoly", 5);
    lendingSystem.addGame("Catan", 3);
    lendingSystem.addGame("Ticket to Ride", 4);

    // Adding customers to the system
    lendingSystem.addCustomer("Alice");
    lendingSystem.addCustomer("Bob");

    // Viewing inventory
    lendingSystem.viewInventory();

    // Checking out games
    lendingSystem.checkOutGame("Monopoly", "Alice");
    lendingSystem.checkOutGame("Catan", "Bob");

    // Viewing borrowed games
    lendingSystem.viewBorrowedGames();

    // Returning games
    lendingSystem.returnGame("Monopoly", "Alice");

    // Viewing inventory after return
    lendingSystem.viewInventory();

    return 0;
}
