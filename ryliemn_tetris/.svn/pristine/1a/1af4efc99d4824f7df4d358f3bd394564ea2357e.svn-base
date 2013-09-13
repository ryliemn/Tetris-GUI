/* TCSS 305 - Spring 2013
 * Tetris Project
 * TetrisGUI Class
 */

package view;

import java.awt.BorderLayout;
import java.awt.Color;
import java.awt.FlowLayout;
import java.awt.event.ActionEvent;
import java.awt.event.ActionListener;
import java.awt.event.KeyAdapter;
import java.awt.event.KeyEvent;
import java.io.File;
import java.io.IOException;
import java.util.Observable;
import java.util.Observer;

import javax.sound.midi.InvalidMidiDataException;
import javax.sound.midi.MidiSystem;
import javax.sound.midi.MidiUnavailableException;
import javax.sound.midi.Sequence;
import javax.sound.midi.Sequencer;
import javax.swing.ButtonGroup;
import javax.swing.JFrame;
import javax.swing.JMenu;
import javax.swing.JMenuBar;
import javax.swing.JMenuItem;
import javax.swing.JOptionPane;
import javax.swing.JRadioButtonMenuItem;
import javax.swing.Timer;

import model.Board;

/**
 * A class that represents the GUI for Tetris.
 * 
 * @author Rylie Nelson
 * @version Spring 2013
 */

public class TetrisGUI extends JFrame implements Observer {

  /**
   * The serial version UID for this class.
   */
  
  private static final long serialVersionUID = 106474944905625785L;
  
  /**
   * The default height of the GUI in pixels.
   */
  
  private static final int DEFAULT_HEIGHT = 235;
  
  /**
   * The default width of the GUI in pixels.
   */
  
  private static final int DEFAULT_WIDTH = 100;
  
  /**
   * The default width of the game board.
   */
  
  private static final int DEFAULT_BOARD_WIDTH = 10;
  
  /**
   * The default height of the game board.
   */
  
  private static final int DEFAULT_BOARD_HEIGHT = 20;
  
  /**
   * The starting delay of the timer.
   */
  
  private static final int DEFAULT_TIMER_DELAY = 1000;
  
  /**
   * The fastest the timer will go.
   */
  
  private static final int TIMER_INCREMENT = 100;
  
  /**
   * The amount of lines a player needs to level up.
   */
  
  private static final int LINES_TO_LEVEL = 10;
  
  /**
   * The timer for this Tetris game.
   */
  
  private final Timer my_game_timer = new Timer(0, null);
  
  /**
   * True if the game is paused, false otherwise.
   */
  
  private boolean my_pause_state;
  
  /**
   * Whether or not the game is currently over.
   */
  
  private boolean my_game_over;
  
  /**
   * The number of lines the player has cleared.
   */
  
  private int my_line_count;
  
  /**
   * Constructs a new TetrisGUI object.
   */
  
  public TetrisGUI() {
    super("Tetris!");
    start();
  }
  
  /**
   * Starts the various components of Tetris.
   */
  
  private void start() {
    setLayout(new FlowLayout());
    final Board board = new Board();
    board.newGame(DEFAULT_BOARD_WIDTH, DEFAULT_BOARD_HEIGHT, null);
    final BoardDisplay board_display = new BoardDisplay();
    final NextPieceDisplay next_piece_display = new NextPieceDisplay();
    final InfoDisplay infodisplay = new InfoDisplay();
    
    my_game_timer.addActionListener(new ActionListener() {
      public void actionPerformed(final ActionEvent the_event) {
        board.step();
      }
    });
    my_game_timer.setDelay(DEFAULT_TIMER_DELAY);
    
    board.addObserver(board_display);
    board.addObserver(next_piece_display);
    board.addObserver(this);
    
    add(board_display, BorderLayout.CENTER);
    add(next_piece_display, BorderLayout.EAST);

    setSize(DEFAULT_WIDTH, DEFAULT_HEIGHT);
    
    getContentPane().setBackground(Color.BLACK);
    setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
    my_game_timer.start();
    setResizable(false);
    
    final TetrisKeyListener listener = new TetrisKeyListener(board, board_display);
    
    board_display.addKeyListener(listener);
    
    createMenuBar(board_display, board, infodisplay);
    
    add(infodisplay);
    
    board.addObserver(infodisplay);
    
    pack();
    
    setVisible(true);
  }
  
  /**
   * Establishes the menu bar for Tetris.
   * 
   * @param the_board_display The display of the board itself.
   * @param the_board The board itself.
   * @param the_info_display The information display.
   */
  
  private void createMenuBar(final BoardDisplay the_board_display, final Board the_board,
                             final InfoDisplay the_info_display) {
    final JMenuBar menubar = new JMenuBar();
    final JMenu file = new JMenu("File");
    final JMenuItem newgame = new JMenuItem("New Game");
    final JMenuItem endgame = new JMenuItem("End Game");
    final JMenu help = new JMenu("Help");
    final JMenuItem controls = new JMenuItem("Controls");
    
    newgame.addActionListener(new ActionListener() {
      public void actionPerformed(final ActionEvent the_event) {
        my_game_timer.stop();
        the_board.newGame(DEFAULT_BOARD_WIDTH, DEFAULT_BOARD_HEIGHT, null);
        my_line_count = 0;
        the_info_display.resetPanel();
        my_game_timer.setDelay(DEFAULT_TIMER_DELAY);
        if (the_board_display.isPaused()) {
          the_board_display.setPause();
        }
        if (the_board_display.isGameOver()) {
          the_board_display.setGameOver();
        }
        my_game_over = false;
        my_game_timer.start();
      }
    });
    
    endgame.addActionListener(new ActionListener() {
      public void actionPerformed(final ActionEvent the_event) {
        my_game_timer.stop();
        the_board_display.setGameOver();
        if (the_board_display.isPaused()) {
          the_board_display.setPause();
        }
        my_game_over = true;
      }
    });
    
    controls.addActionListener(new ActionListener() {
      public void actionPerformed(final ActionEvent the_event) {
        pause(the_board_display);
        printControls();
        pause(the_board_display);
      }
    });
    
    menubar.add(file);
    file.add(newgame);
    file.add(endgame);
    
    createMusicMenu(menubar);
    
    menubar.add(help);
    help.add(controls);
    
    setJMenuBar(menubar);
  }
  
  /**
   * Prints the controls to a pop-up message box.
   */
  
  private void printControls() {
    final StringBuilder sb = new StringBuilder();
    
    sb.append("Left arrow key moves piece left\n");
    sb.append("Right arrow key moves key right\n");
    sb.append("Down arrow key moves key down\n");
    sb.append("Up arrow key hard drops piece\n");
    sb.append("Space bar rotates piece\n");
    sb.append("P pauses your game\n\n");
    sb.append("Level up every ten lines\n");
    sb.append("Good luck!");
    
    JOptionPane.showMessageDialog(null, sb.toString());
  }
  
  /**
   * Toggles the pause state of the game.
   * 
   * @param the_board_display The board display to pause.
   */
  
  private void pause(final BoardDisplay the_board_display) {
    if (my_pause_state) {
      the_board_display.setPause();
      my_game_timer.start();
      my_pause_state = false;
    } else {
      the_board_display.setPause();
      my_game_timer.stop();
      my_pause_state = true;
    }
  }
  
  /**
   * Creates the Music menu and establishes its behavior.
   * 
   * @param the_menubar The menu bar we are adding to.
   */
  
  private void createMusicMenu(final JMenuBar the_menubar) {
    final JMenu music = new JMenu("Music");
    final JRadioButtonMenuItem kissin = new JRadioButtonMenuItem("kissin");
    final JRadioButtonMenuItem flirtin = new JRadioButtonMenuItem("flirtin");
    final JRadioButtonMenuItem dance = new JRadioButtonMenuItem("dance");
    final JRadioButtonMenuItem moteefy = new JRadioButtonMenuItem("moteefy");
    final JRadioButtonMenuItem peripherylol = new JRadioButtonMenuItem("peripherylol");
    final ButtonGroup group = new ButtonGroup();
    
    Sequencer sequencer = null;
    
    Sequence kissinseq = null;
    Sequence flirtinseq = null;
    Sequence danceseq = null;
    Sequence moteefyseq = null;
    Sequence peripherylolseq = null;
    
    try {
      kissinseq = MidiSystem.getSequence(new File("Music files/Tetris songs/kissin.mid"));
      flirtinseq = MidiSystem.getSequence(new File("Music files/Tetris songs/flirtin.mid"));
      danceseq = MidiSystem.getSequence(new File("Music files/Tetris songs/dance.mid"));
      moteefyseq = MidiSystem.getSequence(new File("Music files/Tetris songs/moteefy.mid"));
      peripherylolseq = 
          MidiSystem.getSequence(new File("Music files/Tetris songs/peripherylol.mid"));
      
      sequencer = MidiSystem.getSequencer();
      sequencer.open();
    } catch (final IOException e) {
      System.err.println("IOException caught!");
    } catch (final InvalidMidiDataException e) {
      System.err.println("InvalidMidiDataException caugth!");
    } catch (final MidiUnavailableException e) {
      System.err.println("MidiUnavailableException caught!");
    }
    
    kissin.addActionListener(new SongStarter(kissinseq, sequencer));
    flirtin.addActionListener(new SongStarter(flirtinseq, sequencer));
    dance.addActionListener(new SongStarter(danceseq, sequencer));
    moteefy.addActionListener(new SongStarter(moteefyseq, sequencer));
    peripherylol.addActionListener(new SongStarter(peripherylolseq, sequencer));
    
    group.add(kissin);
    group.add(flirtin);
    group.add(dance);
    group.add(moteefy);
    group.add(peripherylol);
    
    the_menubar.add(music);
    music.add(kissin);
    music.add(flirtin);
    music.add(dance);
    music.add(moteefy);
    music.add(peripherylol);
  }
  
  /**
   * Part of the observer design pattern. Called when the Observables are changed.
   * 
   * @param the_o The observable object.
   * @param the_arg A potentially useful argument.
   */

  @Override
  public void update(final Observable the_o, final Object the_arg) {
    if (the_arg != null) {
      my_line_count = (int) the_arg + my_line_count;
    
      if (DEFAULT_TIMER_DELAY - (my_line_count / LINES_TO_LEVEL * TIMER_INCREMENT) > 0) {
        my_game_timer.setDelay(DEFAULT_TIMER_DELAY 
                               - (my_line_count / LINES_TO_LEVEL * TIMER_INCREMENT));
      } else {
        my_game_timer.setDelay(TIMER_INCREMENT);
      }
    }
    
    if (((Board) the_o).gameIsOver()) {
      my_game_timer.stop();
      JOptionPane.showMessageDialog(null, "SUCCESS WAS NOT YOURS :(");
    }
  }
  
  /**
   * An ActionListener used when the user selects a song.
   * 
   * @author Rylie Nelson
   * @version Spring 2013
   */
  
  private class SongStarter implements ActionListener {
    
    /**
     * The MIDI sequence this ActionListener will start.
     */
    
    private final Sequence my_sequence;
    
    /**
     * The MIDI sequencer used to play the MIDI sequences.
     */
    
    private final Sequencer my_sequencer;
    
    /**
     * Constructs a new SongStarter object.
     * 
     * @param the_sequence The MIDI sequence this SongStarter will play.
     * @param the_sequencer The MIDI sequencer that will play the_sequence.
     */
    
    public SongStarter(final Sequence the_sequence, final Sequencer the_sequencer) {
      my_sequence = the_sequence;
      my_sequencer = the_sequencer;
    }
    
    /**
     * Starts the sequence associated with this SongStarter.
     * 
     * @param the_e The event that has occurred.
     */

    @Override
    public void actionPerformed(final ActionEvent the_e) {
      my_sequencer.stop();
      try {
        my_sequencer.setSequence(my_sequence);
      } catch (final InvalidMidiDataException e) {
        System.err.println("InvalidMidiDataException caught!");
      } 
      my_sequencer.start();
    }
    
  }
  
  /**
   * KeyListener for the Tetris game. Controls movement and pausing.
   * 
   * @author Rylie Nelson
   * @version Spring 2013
   */
  
  private final class TetrisKeyListener extends KeyAdapter {
    
    /**
     * The Board this KeyListener acts upon.
     */
    
    private final Board my_board;
    
    /**
     * The BoardDisplay this KeyListener acts upon.
     */
    
    private final BoardDisplay my_board_display;
    
    /**
     * Constructs a new TetrisKeyListener.
     * 
     * @param the_board The Board this KeyListener acts upon.
     * @param the_board_display The BoardDisplay this KeyListener acts upon.
     */
    
    public TetrisKeyListener(final Board the_board, final BoardDisplay the_board_display) {
      super();
      my_board = the_board;
      my_board_display = the_board_display;
    }
    
    /**
     * Defines the game controls.
     * 
     * @param the_event The event that has occurred.
     */
    
    @Override
    public void keyPressed(final KeyEvent the_event) {
      
      if (!my_pause_state && !my_game_over) {
        if (the_event.getKeyCode() == KeyEvent.VK_DOWN) {
          my_board.moveDown();
        } else if (the_event.getKeyCode() == KeyEvent.VK_LEFT) {
          my_board.moveLeft();
        } else if (the_event.getKeyCode() == KeyEvent.VK_RIGHT) {
          my_board.moveRight();
        } else if (the_event.getKeyCode() == KeyEvent.VK_UP) {
          my_board.hardDrop();
        } else if (the_event.getKeyCode() == KeyEvent.VK_SPACE) {
          my_board.rotate();
        }
      }
      if (the_event.getKeyCode() == KeyEvent.VK_P) {
        pause(my_board_display);
      }
    }
  }

}
