import javax.swing.*;
import java.awt.event.ActionEvent;
import java.awt.event.ActionListener;
import java.time.Duration;
import java.time.Instant;

public class PrimeNumberFinder extends JFrame {
    private JTextField startField;
    private JTextField endField;
    private JTextArea resultArea;
    private JButton findButton;
    private JLabel timeLabel;

    public PrimeNumberFinder() {
        setTitle("Pencari Bilangan Prima");
        setSize(400, 400);
        setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
        setLocationRelativeTo(null);

        JPanel panel = new JPanel();
        panel.setLayout(new BoxLayout(panel, BoxLayout.Y_AXIS));

        panel.add(new JLabel("Nilai Awal:"));
        startField = new JTextField();
        panel.add(startField);

        panel.add(new JLabel("Nilai Akhir:"));
        endField = new JTextField();
        panel.add(endField);

        findButton = new JButton("Cari Bilangan Prima");
        panel.add(findButton);

        resultArea = new JTextArea();
        resultArea.setEditable(false);
        JScrollPane scrollPane = new JScrollPane(resultArea);
        panel.add(scrollPane);

        timeLabel = new JLabel("Total waktu yang dibutuhkan: 0 jam 0 menit 0 detik 0 milidetik");
        panel.add(timeLabel);

        findButton.addActionListener(new ActionListener() {
            @Override
            public void actionPerformed(ActionEvent e) {
                findPrimes();
            }
        });

        add(panel);
    }

    private void findPrimes() {
        try {
            int start = Integer.parseInt(startField.getText());
            int end = Integer.parseInt(endField.getText());
            resultArea.setText("");

            Instant totalStartTime = Instant.now();
            for (int n = start; n <= end; n++) {
                if (isPrime(n)) {
                    resultArea.append("Bilangan prima: " + n + "\n");
                }
            }
            Instant totalEndTime = Instant.now();
            Duration totalElapsedTime = Duration.between(totalStartTime, totalEndTime);

            long millis = totalElapsedTime.toMillis();
            long seconds = millis / 1000;
            long hours = seconds / 3600;
            seconds = seconds % 3600;
            long minutes = seconds / 60;
            seconds = seconds % 60;
            long milliseconds = millis % 1000;
            timeLabel.setText("Total waktu yang dibutuhkan: " + hours + " jam " + minutes + " menit " + seconds + " detik " + milliseconds + " milidetik");

            // Debug statements
            System.out.println("Start Time: " + totalStartTime);
            System.out.println("End Time: " + totalEndTime);
            System.out.println("Elapsed Time: " + totalElapsedTime.toMillis() + " milliseconds");
        } catch (NumberFormatException ex) {
            JOptionPane.showMessageDialog(this, "Masukkan bilangan bulat yang valid.", "Input Error", JOptionPane.ERROR_MESSAGE);
        }
    }

    private boolean isPrime(int n) {
        if (n <= 1) {
            return false;
        }
        for (int i = 2; i <= Math.sqrt(n); i++) {
            if (n % i == 0) {
                return false;
            }
        }
        return true;
    }

    public static void main(String[] args) {
        SwingUtilities.invokeLater(new Runnable() {
            @Override
            public void run() {
                new PrimeNumberFinder().setVisible(true);
            }
        });
    }
}
