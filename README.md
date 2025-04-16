import java.util.ArrayList;
import java.util.HashMap;
import java.util.Map;
import java.util.Scanner;

class Train {
    String name;
    String time;
    int passengerStrength;
    int trainNumber;

    public Train(String name, String time, int passengerStrength, int trainNumber) {
        this.name = name;
        this.time = time;
        this.passengerStrength = passengerStrength;
        this.trainNumber = trainNumber;
    }
}

class ReservationSystem {
    private ArrayList<Train> availableTrains = new ArrayList<>();
    private Map<Integer, ArrayList<String>> bookedSeats = new HashMap<>();

    public ReservationSystem() {
        availableTrains.add(new Train("Mumbai - Delhi", "13:05", 50, 1010));
        availableTrains.add(new Train("Delhi - Jaipur", "7:00", 50, 2013));
        availableTrains.add(new Train("Prayagraj - Delhi", "10:00", 50, 3045));

        // Initialize booking map for each train
        for (Train train : availableTrains) {
            bookedSeats.put(train.trainNumber, new ArrayList<>());
        }
    }

    public void displayAvailableTrains() {
        System.out.println("Available Trains:");
        System.out.println("Train Name\t\tTime\tPassenger Strength\tTrain Number");
        for (Train train : availableTrains) {
            System.out.println(train.name + "\t" + train.time + "\t" + train.passengerStrength + "\t\t" + train.trainNumber);
        }
    }

    public void checkSeatAvailability(int trainNumber) {
        Train train = getTrainByNumber(trainNumber);
        if (train != null) {
            int bookedCount = bookedSeats.get(trainNumber).size();
            int availableSeats = train.passengerStrength - bookedCount;
            System.out.println("Available seats on Train " + trainNumber + ": " + availableSeats);
        } else {
            System.out.println("Train not found.");
        }
    }

    public void bookTicket(int trainNumber, String passengerName) {
        Train train = getTrainByNumber(trainNumber);
        if (train != null) {
            ArrayList<String> passengers = bookedSeats.get(trainNumber);
            if (passengers.size() < train.passengerStrength) {
                passengers.add(passengerName);
                System.out.println("Ticket booked successfully for " + passengerName + " on Train " + trainNumber);
            } else {
                System.out.println("Sorry, the train is fully booked.");
            }
        } else {
            System.out.println("Train not found.");
        }
    }

    public void cancelTicket(int trainNumber, String passengerName) {
        if (bookedSeats.containsKey(trainNumber)) {
            ArrayList<String> passengers = bookedSeats.get(trainNumber);
            if (passengers.remove(passengerName)) {
                System.out.println("Ticket canceled successfully for " + passengerName);
            } else {
                System.out.println("No booking found for passenger: " + passengerName);
            }
        } else {
            System.out.println("Train not found.");
        }
    }

    public void displayBookedTickets(int trainNumber) {
        if (bookedSeats.containsKey(trainNumber)) {
            System.out.println("Booked Tickets for Train " + trainNumber + ":");
            for (String passenger : bookedSeats.get(trainNumber)) {
                System.out.println(passenger);
            }
        } else {
            System.out.println("Train not found.");
        }
    }

    private Train getTrainByNumber(int trainNumber) {
        for (Train train : availableTrains) {
            if (train.trainNumber == trainNumber) {
                return train;
            }
        }
        return null;
    }
}

public class Main {
    public static void main(String[] args) {
        ReservationSystem reservationSystem = new ReservationSystem();
        Scanner scanner = new Scanner(System.in);

        while (true) {
            System.out.println("\nRailway Reservation System Menu:");
            System.out.println("1. Display Available Trains");
            System.out.println("2. Check Seat Availability");
            System.out.println("3. Book a Ticket");
            System.out.println("4. Cancel a Ticket");
            System.out.println("5. Display Booked Tickets");
            System.out.println("6. Exit");
            System.out.print("Enter your choice: ");
            int choice = scanner.nextInt();
            scanner.nextLine();  // Consume newline

            int trainNumber;
            String passengerName;

            switch (choice) {
                case 1:
                    reservationSystem.displayAvailableTrains();
                    break;
                case 2:
                    System.out.print("Enter Train Number: ");
                    trainNumber = scanner.nextInt();
                    reservationSystem.checkSeatAvailability(trainNumber);
                    break;
                case 3:
                    System.out.print("Enter Train Number: ");
                    trainNumber = scanner.nextInt();
                    scanner.nextLine();
                    System.out.print("Enter Passenger Name: ");
                    passengerName = scanner.nextLine();
                    reservationSystem.bookTicket(trainNumber, passengerName);
                    break;
                case 4:
                    System.out.print("Enter Train Number: ");
                    trainNumber = scanner.nextInt();
                    scanner.nextLine();
                    System.out.print("Enter Passenger Name to Cancel: ");
                    passengerName = scanner.nextLine();
                    reservationSystem.cancelTicket(trainNumber, passengerName);
                    break;
                case 5:
                    System.out.print("Enter Train Number: ");
                    trainNumber = scanner.nextInt();
                    reservationSystem.displayBookedTickets(trainNumber);
                    break;
                case 6:
                    System.out.println("Exiting Railway Reservation System. Thank you!");
                    System.exit(0);
                default:
                    System.out.println("Invalid choice. Please try again.");
            }
        }
    }
}
