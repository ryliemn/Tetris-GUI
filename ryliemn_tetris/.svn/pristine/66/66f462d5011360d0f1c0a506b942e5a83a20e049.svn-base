/* TCSS 305 - Spring 2013
 * Tetris Project
 * TetrisMain Class
 */

package view;

import java.awt.EventQueue;

/**
 * A main class that starts the Tetris program.
 * 
 * @author Rylie Nelson
 * @version Spring 2013
 */

public final class TetrisMain {
  
  /**
   * A private constructor to prevent instantiation.
   */
  
  private TetrisMain() {
    //do nothing
  }

  /**
   * Main method; starts Tetris.
   * 
   * @param the_args An array of strings that allows for command line operations; not used.
   */
  
  public static void main(final String[] the_args) {
    EventQueue.invokeLater(new Runnable() {
      @Override
      public void run() {
        new TetrisGUI();
      } 
    });
  }

}
