import javax.swing.*;
import java.awt.*;
import java.awt.event.*;
import java.util.HashMap;

public class TripPlanner {
    private JFrame frame;
    private JPanel currentPanel;
    private HashMap<String, String> userCredentials = new HashMap<>();
    private String currentUser;
    private Color primaryColor = new Color(0, 102, 204); // Blue
    private Color secondaryColor = new Color(255, 153, 0); // Orange
    private Color accentColor = new Color(76, 175, 80); // Green
    private Color backgroundColor = new Color(240, 248, 255); // Alice Blue

    public TripPlanner() {
      // Initialize with some dummy users
        userCredentials.put("user1", "password1");
        userCredentials.put("user2", "password2");

        frame = new JFrame("Travel Planner");
        frame.setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
        frame.setSize(850, 650);
        frame.getContentPane().setBackground(backgroundColor);

        showLoginPanel();

        frame.setVisible(true);
    }

    private void showLoginPanel() {
        currentPanel = new JPanel(new GridBagLayout());
        currentPanel.setBackground(backgroundColor);
        GridBagConstraints gbc = new GridBagConstraints();
        gbc.insets = new Insets(15, 15, 15, 15);

        JLabel titleLabel = new JLabel("✈️ Travel Planner");
        titleLabel.setFont(new Font("Arial", Font.BOLD, 28));
        titleLabel.setForeground(primaryColor);
        gbc.gridx = 0;
        gbc.gridy = 0;
        gbc.gridwidth = 2;
        currentPanel.add(titleLabel, gbc);

        JLabel userLabel = new JLabel("Username:");
        userLabel.setFont(new Font("Arial", Font.BOLD, 14));
        userLabel.setForeground(new Color(70, 70, 70));
        gbc.gridx = 0;
        gbc.gridy = 1;
        gbc.gridwidth = 1;
        currentPanel.add(userLabel, gbc);

        JTextField userText = new JTextField(15);
        userText.setFont(new Font("Arial", Font.PLAIN, 14));
        userText.setBorder(BorderFactory.createCompoundBorder(
                BorderFactory.createLineBorder(primaryColor, 1),
                BorderFactory.createEmptyBorder(5, 5, 5, 5)));
        gbc.gridx = 1;
        gbc.gridy = 1;
        currentPanel.add(userText, gbc);

        JLabel passwordLabel = new JLabel("Password:");
        passwordLabel.setFont(new Font("Arial", Font.BOLD, 14));
        passwordLabel.setForeground(new Color(70, 70, 70));
        gbc.gridx = 0;
        gbc.gridy = 2;
        currentPanel.add(passwordLabel, gbc);

        JPasswordField passwordText = new JPasswordField(15);
        passwordText.setFont(new Font("Arial", Font.PLAIN, 14));
        passwordText.setBorder(BorderFactory.createCompoundBorder(
                BorderFactory.createLineBorder(primaryColor, 1),
                BorderFactory.createEmptyBorder(5, 5, 5, 5)));
        gbc.gridx = 1;
        gbc.gridy = 2;
        currentPanel.add(passwordText, gbc);

        JButton loginButton = createStyledButton("Login", primaryColor);
        gbc.gridx = 0;
        gbc.gridy = 3;
        gbc.gridwidth = 2;
        loginButton.addActionListener(new ActionListener() {
            public void actionPerformed(ActionEvent e) {
                String username = userText.getText();
                String password = new String(passwordText.getPassword());

                if (userCredentials.containsKey(username)) {
                    if (userCredentials.get(username).equals(password)) {
                        currentUser = username;
                        showTripPlannerPanel();
                    } else {
                        JOptionPane.showMessageDialog(frame, "Invalid password");
                    }
                } else {
                    JOptionPane.showMessageDialog(frame, "Username not found");
                }
            }
        });
        currentPanel.add(loginButton, gbc);

        JButton registerButton = createStyledButton("Register", secondaryColor);
        registerButton.addActionListener(new ActionListener() {
            public void actionPerformed(ActionEvent e) {
                String username = userText.getText();
                String password = new String(passwordText.getPassword());

                if (username.isEmpty() || password.isEmpty()) {
                    JOptionPane.showMessageDialog(frame, "Username and password cannot be empty");
                    return;
                }

                if (userCredentials.containsKey(username)) {
                    JOptionPane.showMessageDialog(frame, "Username already exists");
                } else {
                    userCredentials.put(username, password);
                    JOptionPane.showMessageDialog(frame, "Registration successful! Please login.");
                }
            }
        });
        gbc.gridy = 4;
        currentPanel.add(registerButton, gbc);

        frame.setContentPane(currentPanel);
        frame.revalidate();
    }

    private JButton createStyledButton(String text, Color bgColor) {
        JButton button = new JButton(text);
        button.setFont(new Font("Arial", Font.BOLD, 14));
        button.setBackground(bgColor);
        button.setForeground(Color.WHITE);
        button.setFocusPainted(false);
        button.setBorder(BorderFactory.createEmptyBorder(10, 20, 10, 20));
        button.setCursor(new Cursor(Cursor.HAND_CURSOR));

        // Add hover effect
        button.addMouseListener(new MouseAdapter() {
            public void mouseEntered(MouseEvent e) {
                button.setBackground(bgColor.darker());
            }

            public void mouseExited(MouseEvent e) {
                button.setBackground(bgColor);
            }
        });

        return button;
    }

    private void showTripPlannerPanel() {
        currentPanel = new JPanel(new GridBagLayout());
        currentPanel.setBackground(backgroundColor);
        GridBagConstraints gbc = new GridBagConstraints();
        gbc.insets = new Insets(15, 15, 15, 15);
        gbc.fill = GridBagConstraints.HORIZONTAL;

        JLabel welcomeLabel = new JLabel("Welcome, " + currentUser + "! ✈️ Plan Your Trip");
        welcomeLabel.setFont(new Font("Arial", Font.BOLD, 22));
        welcomeLabel.setForeground(primaryColor);
        gbc.gridx = 0;
        gbc.gridy = 0;
        gbc.gridwidth = 2;
        currentPanel.add(welcomeLabel, gbc);

        // Destination
        JLabel destLabel = createFormLabel("Destination:");
        gbc.gridx = 0;
        gbc.gridy = 1;
        gbc.gridwidth = 1;
        currentPanel.add(destLabel, gbc);

        JTextField destText = createStyledTextField();
        gbc.gridx = 1;
        gbc.gridy = 1;
        currentPanel.add(destText, gbc);

        // Number of Days
        JLabel daysLabel = createFormLabel("Number of Days:");
        gbc.gridx = 0;
        gbc.gridy = 2;
        currentPanel.add(daysLabel, gbc);

        JSpinner daysSpinner = createStyledSpinner(1, 1, 30);
        gbc.gridx = 1;
        gbc.gridy = 2;
        currentPanel.add(daysSpinner, gbc);

        // Total Budget (converted to rupees)
        JLabel budgetLabel = createFormLabel("Total Budget (₹):");
        gbc.gridx = 0;
        gbc.gridy = 3;
        currentPanel.add(budgetLabel, gbc);

        // Default value converted to rupees (1 USD ≈ 83 INR)
        JSpinner budgetSpinner = createStyledSpinner(500 * 83, 100 * 83, 100000 * 83);
        gbc.gridx = 1;
        gbc.gridy = 3;
        currentPanel.add(budgetSpinner, gbc);

        // Number of People
        JLabel peopleLabel = createFormLabel("Number of People:");
        gbc.gridx = 0;
        gbc.gridy = 4;
        currentPanel.add(peopleLabel, gbc);

        JSpinner peopleSpinner = createStyledSpinner(1, 1, 20);
        gbc.gridx = 1;
        gbc.gridy = 4;
        currentPanel.add(peopleSpinner, gbc);

        // Plan Trip Button
        JButton planButton = createStyledButton("Plan My Trip ✈️", accentColor);
        planButton.addActionListener(new ActionListener() {
            public void actionPerformed(ActionEvent e) {
                String destination = destText.getText();
                int days = (Integer) daysSpinner.getValue();
                double budget = (Integer) budgetSpinner.getValue(); // Already in rupees
                int people = (Integer) peopleSpinner.getValue();

                if (destination.isEmpty()) {
                    JOptionPane.showMessageDialog(frame, "Please enter a destination");
                    return;
                }

                showTripDetailsPanel(destination, days, budget, people);
            }
        });
        gbc.gridx = 0;
        gbc.gridy = 5;
        gbc.gridwidth = 2;
        currentPanel.add(planButton, gbc);

        // Logout Button
        JButton logoutButton = createStyledButton("Logout", new Color(200, 50, 50));
        logoutButton.addActionListener(new ActionListener() {
            public void actionPerformed(ActionEvent e) {
                currentUser = null;
                showLoginPanel();
            }
        });
        gbc.gridy = 6;
        currentPanel.add(logoutButton, gbc);

        frame.setContentPane(currentPanel);
        frame.revalidate();
    }

    private JLabel createFormLabel(String text) {
        JLabel label = new JLabel(text);
        label.setFont(new Font("Arial", Font.BOLD, 14));
        label.setForeground(new Color(70, 70, 70));
        return label;
    }

    private JTextField createStyledTextField() {
        JTextField field = new JTextField(20);
        field.setFont(new Font("Arial", Font.PLAIN, 14));
        field.setBorder(BorderFactory.createCompoundBorder(
                BorderFactory.createLineBorder(primaryColor, 1),
                BorderFactory.createEmptyBorder(8, 8, 8, 8)));
        return field;
    }

    private JSpinner createStyledSpinner(int value, int min, int max) {
        JSpinner spinner = new JSpinner(new SpinnerNumberModel(value, min, max, min == 1 ? 1 : 1000));
        spinner.setFont(new Font("Arial", Font.PLAIN, 14));
        spinner.setBorder(BorderFactory.createCompoundBorder(
                BorderFactory.createLineBorder(primaryColor, 1),
                BorderFactory.createEmptyBorder(5, 5, 5, 5)));
        return spinner;
    }

    private void showTripDetailsPanel(String destination, int days, double budget, int people) {
        currentPanel = new JPanel(new BorderLayout());
        currentPanel.setBackground(backgroundColor);

        JPanel detailsPanel = new JPanel(new GridLayout(0, 1, 10, 10));
        detailsPanel.setBorder(BorderFactory.createEmptyBorder(20, 20, 20, 20));
        detailsPanel.setBackground(backgroundColor);

        // Calculate trip details
        double dailyBudget = budget / days;
        double perPersonBudget = budget / people;

        // Sample accommodations (converted to rupees)
        String[] accommodations = {
                "🏨 Luxury Hotel (₹" + (200 * 83) + "/night)",
                "🏨 Mid-range Hotel (₹" + (100 * 83) + "/night)",
                "🏨 Budget Hotel (₹" + (50 * 83) + "/night)",
                "🛏️ Hostel (₹" + (20 * 83) + "/night)"
        };

        // Sample attractions (converted to rupees)
        String[] attractions = {
                "🏙️ City Center (Free)",
                "🏛️ Local Museum (₹" + (10 * 83) + " entry)",
                "🗼 Famous Landmark (₹" + (15 * 83) + " entry)",
                "🌳 Nature Park (Free)",
                "🚶 Guided Tour (₹" + (25 * 83) + " per person)"
        };

        // Create trip details text with colorful formatting
        JTextPane detailsText = new JTextPane();
        detailsText.setEditable(false);
        detailsText.setBackground(new Color(255, 255, 240)); // Light yellow
        detailsText.setBorder(BorderFactory.createEmptyBorder(10, 15, 10, 15));

        // Create styled document
        javax.swing.text.StyledDocument doc = detailsText.getStyledDocument();
        javax.swing.text.Style defaultStyle = detailsText.getStyle(javax.swing.text.StyleContext.DEFAULT_STYLE);
        javax.swing.text.Style boldStyle = detailsText.addStyle("bold", defaultStyle);
        javax.swing.text.StyleConstants.setBold(boldStyle, true);
        javax.swing.text.Style colorStyle = detailsText.addStyle("color", defaultStyle);
        javax.swing.text.StyleConstants.setForeground(colorStyle, primaryColor);

        try {
            // Header
            doc.insertString(doc.getLength(), "TRIP DETAILS FOR " + destination.toUpperCase() + "\n\n", boldStyle);

            // Budget info
            doc.insertString(doc.getLength(), "Duration: " + days + " days\n", defaultStyle);
            doc.insertString(doc.getLength(), "Total Budget: ₹" + String.format("%.2f", budget) +
                    " (₹" + String.format("%.2f", dailyBudget) + " per day, ₹" +
                    String.format("%.2f", perPersonBudget) + " per person)\n\n", defaultStyle);

            // Accommodations
            doc.insertString(doc.getLength(), "ACCOMMODATION OPTIONS:\n", colorStyle);
            for (String acc : accommodations) {
                doc.insertString(doc.getLength(), "• " + acc + "\n", defaultStyle);
            }

            doc.insertString(doc.getLength(), "\n", defaultStyle);

            // Attractions
            doc.insertString(doc.getLength(), "ATTRACTIONS TO VISIT:\n", colorStyle);
            for (String attr : attractions) {
                doc.insertString(doc.getLength(), "• " + attr + "\n", defaultStyle);
            }

            doc.insertString(doc.getLength(), "\n", defaultStyle);

            // Itinerary
            doc.insertString(doc.getLength(), "RECOMMENDED ITINERARY:\n", colorStyle);
            if (days >= 3) {
                doc.insertString(doc.getLength(), "• Day 1: Arrival, check in to accommodation, explore City Center\n",
                        defaultStyle);
                doc.insertString(doc.getLength(),
                        "• Day 2: Visit Famous Landmark in morning, Local Museum in afternoon\n", defaultStyle);
                doc.insertString(doc.getLength(), "• Day 3: Nature Park and Guided Tour\n", defaultStyle);
            } else {
                doc.insertString(doc.getLength(), "• Day 1: Arrival, check in, visit City Center and Famous Landmark\n",
                        defaultStyle);
                doc.insertString(doc.getLength(), "• Day 2: Local Museum and free exploration\n", defaultStyle);
            }
        } catch (Exception e) {
            e.printStackTrace();
        }

        JScrollPane scrollPane = new JScrollPane(detailsText);
        scrollPane.setBorder(BorderFactory.createLineBorder(primaryColor, 1));
        detailsPanel.add(scrollPane);

        JButton backButton = createStyledButton("← Back to Planner", secondaryColor);
        backButton.addActionListener(new ActionListener() {
            public void actionPerformed(ActionEvent e) {
                showTripPlannerPanel();
            }
        });

        JPanel buttonPanel = new JPanel();
        buttonPanel.setBackground(backgroundColor);
        buttonPanel.add(backButton);

        currentPanel.add(detailsPanel, BorderLayout.CENTER);
        currentPanel.add(buttonPanel, BorderLayout.SOUTH);

        frame.setContentPane(currentPanel);
        frame.revalidate();
    }

    public static void main(String[] args) {
        SwingUtilities.invokeLater(new Runnable() {
            public void run() {
                new TripPlanner();
            }
        });
    }
}
