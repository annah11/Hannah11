# Hannah11
import javax.swing.JButton;
import javax.swing.JFrame;
import javax.swing.JOptionPane;
import javax.swing.JPanel;
import javax.swing.SwingUtilities;
import javax.swing.border.EmptyBorder;
import java.awt.Dimension;
import java.awt.FlowLayout;
import java.awt.Font;
import java.awt.GridLayout;
import java.awt.event.ActionEvent;
import java.awt.event.ActionListener;
import javax.swing.border.Border;
import javax.swing.border.EmptyBorder;
import java.awt.Insets;


public class TicTacToeGame {
    private JFrame frame;
    private JButton[] buttons;
    private boolean isPlayerX;

    public TicTacToeGame() {
        frame = new JFrame("Tic Tac Toe");
        frame.setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
        


        // Create a panel with a grid layout
        //=======================================
        JPanel panel = new JPanel(new GridLayout(3, 3));
        panel.setPreferredSize(new Dimension(50, 50));
        buttons = new JButton[9];
        isPlayerX = true;





        // Create and add buttons to the panel
        //=====================================
        for (int i = 0; i < 9; i++) {
            buttons[i] = new JButton();
            buttons[i].addActionListener(new ButtonClickListener(i));

            buttons[i].setFont(new Font(buttons[i].getFont().getName(), Font.BOLD, 64)); 
            panel.add(buttons[i]);
            
        }




        // Add the panel to the frame
        //============================
        frame.add(panel);

        frame.pack();
        frame.setLocationRelativeTo(null);
        frame.setSize(300, 200);
        frame.setVisible(true);
    }

    private class ButtonClickListener implements ActionListener {
        private int buttonIndex;

        public ButtonClickListener(int index) {
            buttonIndex = index;
        }

        @Override
        public void actionPerformed(ActionEvent e) {
            JButton button = buttons[buttonIndex];

            // Check if the button is already clicked
            if (button.getText().isEmpty()) {
                button.setText(isPlayerX ? "X" : "O");
                isPlayerX = !isPlayerX;
                checkWinner();
            }
        }

        private void checkWinner() {


            // Define winning combinations of row , column  and diagonal
            //================================================================
            int[][] winningCombinations = {
                    {0, 1, 2}, {3, 4, 5}, {6, 7, 8}, 
                    {0, 3, 6}, {1, 4, 7}, {2, 5, 8}, 
                    {0, 4, 8}, {2, 4, 6}             
            };

            // Check for a winning combination
            //==================================
            for (int[] combination : winningCombinations) {
                int a = combination[0];
                int b = combination[1];
                int c = combination[2];

                if (buttons[a].getText().equals(buttons[b].getText()) &&
                        buttons[b].getText().equals(buttons[c].getText()) &&
                        !buttons[a].getText().isEmpty()) {



                            
                    // We have a winner
                    //===================
                    String winner = buttons[a].getText();
                    JOptionPane.showMessageDialog(frame, "Player " + winner + " wins!");
                    resetGame();
                    return;
                }
            }

        
            boolean isTie = true;
            for (JButton button : buttons) {
                if (button.getText().isEmpty()) {
                    isTie = false;
                    break;
                }
            }

            if (isTie) {
        
                JOptionPane.showMessageDialog(frame, "It's a tie!");
                resetGame();
            }
        }

        private void resetGame() {
            for (JButton button : buttons) {
                button.setText("");
            }
            isPlayerX = true;
        }
    }

    public static void main(String[] args) {
        SwingUtilities.invokeLater(new Runnable() {
            @Override
            public void run() {
                new TicTacToeGame();
            }
        });
    }
}

