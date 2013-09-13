/* TCSS 305 - Spring 2013
 * Tetris Project
 * InfoDisplay class
 */

package view;

import java.awt.Color;
import java.awt.Dimension;
import java.awt.Graphics;
import java.awt.Graphics2D;
import java.awt.RenderingHints;
import java.util.Observable;
import java.util.Observer;

import javax.swing.BorderFactory;
import javax.swing.JPanel;

/**
 * Displays level and score information.
 * 
 * @author Rylie Nelson
 * @version Spring 2013
 */

public class InfoDisplay extends JPanel implements Observer {
  
  /**
   * The serial version UID of this class.
   */
  
  private static final long serialVersionUID = 8827200647289008554L;
  
  /**
   * Centers the text in the x coordinate.
   */
  
  private static final int TEXT_X_COORDINATE = 25;
  
  /**
   * The location of the first line of text.
   */
  
  private static final int LINE1_Y = 25;
  
  /**
   * The location of the second line of text.
   */
  
  private static final int LINE2_Y = 50;
  
  /**
   * The location of the third line of text.
   */
  
  private static final int LINE3_Y = 75;

  /**
   * The default height of the InfoDisplay in pixels.
   */
  
  private static final int DEFAULT_HEIGHT = 100;
  
  /**
   * The default width of the InfoDisplay in pixels.
   */
  
  private static final int DEFAULT_WIDTH = 150;
  
  /**
   * The amount of lines a player needs to level up.
   */
  
  private static final int LINES_TO_LEVEL = 10;
  
  /**
   * The current level of the player's game. Ten cleared lines moves the player up a level.
   * Speed of the falling block increments down by 100ms per level until it reaches 100ms.
   */
  
  private int my_level;
  
  /**
   * The current score of the player's game. Score for a given line clear is calculated by
   * simply squaring the number of cleared lines.
   */
  
  private int my_score;
  
  /**
   * The number of lines the player has cleared.
   */
  
  private int my_line_count;
  
  /**
   * Constructs a new InfoDisplay object.
   */
  
  public InfoDisplay() {
    super();
    start();
    my_level = 1;
    my_score = 0;
  }
  
  /**
   * Sets up various aesthetic aspects of the panel.
   */
  
  private void start() {
    setPreferredSize(new Dimension(DEFAULT_WIDTH, DEFAULT_HEIGHT));
    setBorder(BorderFactory.createLineBorder(Color.WHITE));
    setBackground(Color.BLACK);
  }
  
  /**
   * Resets the line count, level, and score.
   */
  
  public void resetPanel() {
    my_line_count = 0;
    my_level = 1;
    my_score = 0;
    repaint();
  }
  
  /**
   * Paints the contents on the panel.
   * 
   * @param the_graphics The graphic to be drawn.
   */
  
  @Override
  public void paintComponent(final Graphics the_graphics) {
    super.paintComponent(the_graphics);
    final Graphics2D g2d = (Graphics2D) the_graphics;
    g2d.setRenderingHint(RenderingHints.KEY_ANTIALIASING, RenderingHints.VALUE_ANTIALIAS_ON);
    g2d.setPaint(Color.WHITE);
    
    g2d.drawString("Current Level: " + my_level, TEXT_X_COORDINATE, LINE1_Y);
    g2d.drawString("Lines Cleared: " + my_line_count, TEXT_X_COORDINATE, LINE2_Y);
    g2d.drawString("Current Score: " + my_score, TEXT_X_COORDINATE, LINE3_Y);
  }

  @Override
  public void update(final Observable the_o, final Object the_arg) {
    if (the_arg != null) {
      my_line_count = my_line_count + (int) the_arg;
      my_level = my_line_count / LINES_TO_LEVEL + 1;
      my_score = my_score + ((int) the_arg * (int) the_arg);
      repaint();
    }
  }
  

}
