class Counter {
    private int count = 0;

    // Increment method without synchronization (may cause interference)
    public void increment() {
        count++;
    }

    // Synchronized increment method to prevent interference
    public synchronized void synchronizedIncrement() {
        count++;
    }

    // Getter for count
    public int getCount() {
        return count;
    }
}

public class InterferenceExample {
    public static void main(String[] args) throws InterruptedException {
        Counter counter = new Counter();

        // Create two threads that modify the counter
        Thread t1 = new Thread(() -> {
            for (int i = 0; i < 1000; i++) {
                counter.increment(); // This may cause interference
            }
        });

        Thread t2 = new Thread(() -> {
            for (int i = 0; i < 1000; i++) {
                counter.increment(); // This may cause interference
            }
        });

        // Start the threads
        t1.start();
        t2.start();

        // Wait for both threads to complete
        t1.join();
        t2.join();

        // Display the final count
        System.out.println("Count without synchronization: " + counter.getCount());

        // Reset count and use synchronizedIncrement
        Counter synchronizedCounter = new Counter();

        Thread t3 = new Thread(() -> {
            for (int i = 0; i < 1000; i++) {
                synchronizedCounter.synchronizedIncrement();
            }
        });

        Thread t4 = new Thread(() -> {
            for (int i = 0; i < 1000; i++) {
                synchronizedCounter.synchronizedIncrement();
            }
        });

        // Start the threads
        t3.start();
        t4.start();

        // Wait for both threads to complete
        t3.join();
        t4.join();

        // Display the final count
        System.out.println("Count with synchronization: " + synchronizedCounter.getCount());
    }
}
