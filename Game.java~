/*----------------------------------------------------------------
 *  Author:   Apurva Gandhi
 *  Email:    ahgand22@g.holycross.edu
 *  Written:  11/16/2018
 *  Last Updated: 01/12/2018
 *  
 *  Minesweeper game. This class implements the game window and most
 *  of the baisc game logic.
 *
 ****  Extra Credit/features:
 *  1. Auto-complete: When a blank cell is revealed, all neighboring cells are also revealed.
 *     If any of those newly revealed cells are blank, then their neighbors will  be revealed too,
 *     and so on until the neighbors of every newly revealed blank cell are revealed.
 *  2. Artistic effort: used beveled edges on the cells. Draws numbers in appropriate colors,
 *     shows grid lines between boxes, uses images to show mines and flags
 *  3. Nice game ending: when the game is won or lost, the game includes a special user-friendly message
 *     providing the current game-winning or losing status
 *     It also prints out Victory Royale and Winner winner chicken dinner which is the famous tag
 *     lines for the current trending game, PUBG, and Fortnite 
 *  4. Flags: right-clicking on unrevealed cell plants or removes a flag on that cell.
 *     Left click on a flag does nothing. If the user has put the correct number of flags, 
 *     every flag is on a hidden mine, and no flag is on a cell that is not a mine,
 *     then the game is considered a win. Flags are shown using an imported image of the Indian flag.
 *  5. Game save/restore: user can press' s' key be "save", and 'r' key to be "restore".
 *     Saves and restores the game. The user can save the game of any difficulty and come back
 *     later and press 'r' key to restore the saved game
 *     The help box is also revised with information to save and restore keys for the better user-friendly experience.
 *  IN ADDITION, if user presses 's' key after winning or losing the game, it will print out 
 *     a message stating that they can not save the game because they have already won or lost.
 *     When they try to restore a game after trying to save a won or lost game, 
 *     It will show the label which states that they can not restore a game because
 *     they tried to save already won or lost game last time. 
 *     It will also print out that this is a new game and enjoy the new game
 *----------------------------------------------------------------*/

import GUI.*;

/**
 * A <i>Game</i> object manages all information about a minesweeper game as it
 * is being played and displayed on the screen. This includes information about
 * all of the cells (this is stored in a 2-D array of Cell objects), how many
 * flags have been planted, how many mines have been deployed, etc. Game extends
 * Window, so it can be drawn on the screen. It also extends EventListener so it
 * can respond to user interaction.
 */
public class Game extends Window implements EventListener
{

    /**
     * Number of cells tall the game board will be.
     */
    public static final int NUM_ROWS = 20;

    /**
     * Number of cells wide the game board will be.
     */
    public static final int NUM_COLS = 30;

    // Example game screen layout:
    // +---------------------------------------------------------+
    // |      M A R G I N = 50                                   |
    // | M  + - - - - - - - - - - - - - - - - - - - - - - - + M  |
    // | A  |                                               | A  |
    // | R  |                                               | R  |
    // | G  |                Grid of Cells                  | G  |
    // | I  |                                               | I  |
    // | N  |                                               | N  |
    // | =  |       600 = NUM_COLS * Cell.SIZE wide         | =  |
    // | 50 |                      by                       | 50 |
    // |    |       400 = NUM_ROWS * Cell.SIZE tall         |    |
    // |    |                                               |    |
    // |    |                                               |    |
    // |    |                                               |    |
    // |    + - - - - - - - - - - - - - - - - - - - - - - - +    |
    // |            SPACE     S   SPACE   S    SPACE             |
    // |    + - - - - - - - + P + - - - + P + - - - - - - - +    |
    // |    |    Status     | A | Timer | A |     Help      |    |
    // |    |       Box     | C |       | C |      Box      |    |
    // |    + - - - - - - - + E + - - - + E + - - - - - - - +    |
    // |     M A R G I N = 50                                    |
    // +-- ------------------------------------------------------+

    /**
     * Width of the game window, in pixels.
     * Equal to 2*MARGIN + GRID_WIDTH
     * or 2*MARGIN + 2*SPACE + StatusBox.WIDTH, Timer.WIDTH, HelpBox.WIDTH,
     * whichever is larger.
     */
    public static final int WIDTH = 700;

    /**
     * Height of the game window, in pixels.
     * Equal to 2*MARGIN + SPACE
     *     + GRID_HEIGHT
     *     + max(StatusBox.Height, Timer.HEIGHT, HelpBox.HEIGHT)
     */
    public static final int HEIGHT = 600; 

    /**
     * Width of the grid part of the window, in pixels.
     * Equal to NUM_COLS * Cell.SIZE.
     */
    public static final int GRID_WIDTH = NUM_COLS * Cell.SIZE;

    /**
     * Height of the grid part of the window, in pixels.
     * Equal to NUM_ROWS * Cell.SIZE.
     */
    public static final int GRID_HEIGHT = NUM_ROWS * Cell.SIZE;

    /**
     * Margin around the edges of the canvas.
     */
    private static final int MARGIN = 50;

    /**
     * Space between elements on the canvas.
     */
    private static final int SPACE = 25;



    // A 2-D array of Cell objects to keep track of the board state.
    private Cell[][] cells = new Cell[NUM_ROWS][NUM_COLS];

    private int numMines = 0;    // number of mines deployed
    private int numRevealed = 0; // number of cells revealed so far

    private int flagMine = 0; //number of flags are on a mine
    private int totalFlag = 0; //number of toal flags


    // Whether or not the game has been won.
    private boolean gameWon = false;

    // Whether or not the game has been lost
    private boolean gameLost = false;

    // Name of the user playing the game.
    private String username;

    // The difficulty level of the game, used for tracking top scores.
    private String difficulty;

    // The status box that appears in the top left.
    private StatusBox status;

    // The timer that appears in the top middle.
    private Timer timer;

    // The help box that appears in the top right.
    private HelpBox help;

    /**
     * Constructor: Initializes a new game, but does not deploy any mines, plant
     * any flags, etc. The difficulty is either "easy", "medium", or "hard", and
     * will be used to load the proper top scores file. Name is used as the
     * user's name.
     */
    public Game(String name, String difficulty)
    {
        super("Minesweeper!", WIDTH, HEIGHT);

        this.username = name;
        this.difficulty = difficulty;

        // Create the background
        setBackgroundColor(Canvas.DARK_GRAY);

        // Create a border around the grid
        Box border = new Box(MARGIN-1.5, MARGIN-1.5, GRID_WIDTH+3, GRID_HEIGHT+3);
        border.setBackgroundColor(null);
        border.setBorderColor(Canvas.BLACK);
        add(border);

        // Create the info boxes
        help = new HelpBox(WIDTH - MARGIN - HelpBox.WIDTH,HEIGHT - MARGIN - HelpBox.HEIGHT);
        add(help);
        timer = new Timer( WIDTH - MARGIN - HelpBox.WIDTH - Timer.WIDTH - SPACE, HEIGHT - MARGIN - HelpBox.HEIGHT);
        add(timer);
        timer.reset(0.0);  
        status = new StatusBox(this, (WIDTH - MARGIN - HelpBox.WIDTH - Timer.WIDTH - StatusBox.WIDTH - 2*SPACE), HEIGHT - MARGIN - HelpBox.HEIGHT);
        add(status);

        //Creates 600 new cells based on the rows and columns
        for(int row = 0; row < NUM_ROWS; row++)
        {
            for(int col = 0; col < NUM_COLS; col++)
            {
                cells[row][col] = new Cell (MARGIN+Cell.SIZE*col, MARGIN+Cell.SIZE*row);
                add(cells[row][col]);
            }
        }
    }

    /**
     * Get the number of mines that are deployed.
     */
    public int getNumMinesDeployed()
    {
        return numMines;
    }

    /**
     * Get the number of hidden cells remaining to be revealed.
     */
    public int getNumCellsRemaining()
    {
        return NUM_ROWS * NUM_COLS - numRevealed;
    }
   
    /**
     * Deploy the given number of mines. This gets called once during game
     * setup. The game doesn't actually begin officially until the user clicks
     * a cell, so the timer should not start yet.
     */
   
    public void deployMines(int mines)
    {
        //Deployed the Mines randomely to the grid
        /**
         *It will first select random numbers and make a mine at that
         * coordinates, Later it will check if there is a mine at those
         *coordinates and if there is not mine, it will plant a mine there.
         **/
        do
        {
            int row = StdRandom.uniform(NUM_ROWS);
            int col = StdRandom.uniform(NUM_COLS);

            if(row >= 0 || row < NUM_ROWS || col < NUM_COLS || col >= 0)
            {
                if((cells[row][col].isMine() == false))
                {
                    cells[row][col].makeMine();
                    numMines++;
                }
            }
        }// end of do While
        
        while (numMines < mines);
        
        /**
         *Initialies the neighboring mine count by going through
         *each cell and checking its out of grid conditions
         *and increment its neighbor mine count if there is a mine.
         **/
        for(int row = 0; row < NUM_ROWS; row++)
        {
            for (int col = 0;col < NUM_COLS; col++)
            {
                if(cells[row][col].isMine())
                {
                    //Right to the mine
                    if (row >= 0 && col < NUM_COLS-1)
                    {
                        cells[row][col+1].incrementNeighborMineCount();
                    }
                    //Down to the mine
                    if(col >= 0 && row < NUM_ROWS-1)
                    {
                        cells[row+1][col].incrementNeighborMineCount();
                    }
                    //Top of the mine
                    if(col >=0 && row > 0)
                    {
                        cells[row-1][col].incrementNeighborMineCount();
                    }
                    //Left to the mine
                    if (row >= 0 && col > 0)
                    {
                        cells[row][col-1].incrementNeighborMineCount();
                    }
                    //Bottom right corner
                    if(row < NUM_ROWS-1 && col < NUM_COLS-1)
                    {
                        cells[row+1][col+1].incrementNeighborMineCount();
                    }
                    //Top left corner
                    if(row > 0 && col > 0)
                    {
                        cells[row-1][col-1].incrementNeighborMineCount();
                    }
                    //Bottom left corner
                    if(row < NUM_ROWS-1 && col > 0)
                    {
                        cells[row+1][col-1].incrementNeighborMineCount();
                    }
                    //Top right corner
                    if(row > 0 && col < NUM_COLS-1)
                    {
                        cells[row-1][col+1].incrementNeighborMineCount();
                    }
                }//end of outer if condition
            } //end of inner for loop 
        }//end of outer for loop
    }//end of deployMines
    
    /**
     *revealZeros is called when user clicked a mouse click
     *It is recursive function calling itself to rereveal
     *all the surrouning cells with no mines. It will 
     *stop when there are mine surrounding the cells
     *and if it goes out of the grid
     * @param row the row at which the mouse is clicked.
     * @param col the column at which mouse is clicked.
     **/
    public  void revealZeros (int row, int col)
    {
        //Base Case
        if(row < 0 || row > NUM_ROWS-1 || col < 0 || col > NUM_COLS-1 || cells[row][col].isRevealed() ||  cells[row][col].isMine())
        {
            return;
        }
        //General Case
        //It will reveal the current cell and increment number
        //of cells revealed.
        cells[row][col].reveal();
        numRevealed++;
        //If the current cell is not a mine and surrounding has no mine
        //it will call itself recursively
        if(cells[row][col].getNeighborMineCount() == 0)
        {
            ///Right
            revealZeros(row,col+1);
            //Left
            revealZeros(row,col-1);
            //Bottom
            revealZeros(row+1,col);
            //Top
            revealZeros(row-1,col);
        }        
    }//end of revealZeroes
         
    /**
     * Respond to a mouse click. This function will be called each time the user
     * clicks on the game window. The x, y parameters indicate the screen
     * coordinates where the user has clicked, and the button parameter
     * indicates which mouse button was clicked (either "left", "middle", or
     * "right"). The function should update the game state according to what the
     * user has clicked.
     * @param x the x coordinate where the user clicked, in pixels.
     * @param y the y coordinate where the user clicked, in pixels.
     * @param button either "left", "middle", or "right".
     **/   
    public void mouseClicked(double x, double y, String button)
    {
        // If game is over, then ignore the mouse click.
        if (gameWon || gameLost)
            return;

        // If the user middle-clicked, ignore it.
        if (!button.equals("left") && !button.equals("right"))
            return;

        // If the user clicked outside of the game grid, ignore it.
        if (x < MARGIN || y < MARGIN  || x >= MARGIN + GRID_WIDTH || y >= MARGIN + GRID_HEIGHT)
        {
            return;
        }

        // Calculate which cell the user clicked.
        int row = (int)((y - MARGIN) / Cell.SIZE);
        int col = (int)((x - MARGIN) / Cell.SIZE);

        //Starts the timer
        timer.startCounting();

        //when user presses left click
        if (button.equals("left"))
        {
            //Reveals the Cell if not already revealed and there is no flag
            if(!cells[row][col].isRevealed() &&  !cells[row][col].isFlag())

            {
                //calls the recursive function to reveal current and
                //other zeroes
                revealZeros(row, col);             
            }
            
            //If clicked on Mine, and there is a mine and no flag
            //it will Reveal all the mines and delare Lost 
            if(cells[row][col].isMine() && !cells[row][col].isFlag())
            {
                for(int i = 0; i < NUM_ROWS; i++)
                {
                    for(int j = 0; j < NUM_COLS; j++)
                    {
                        //Revelaed the all cells with mines to the screen
                        cells[i][j].showMine();
                    }
                }
                //Stops the timer
                timer.stopCounting();
                //Decalres the game as lost
                gameLost = true;
            }
        }//end of left click pressed
        //when user presses right click
        else if(button.equals("right"))
        {
            /**
             *if there is no flag, and cell is not revealed
             *it will create a flag and increment total flag count;
             **/
            if(!(cells[row][col].isFlag()) && !cells[row][col].isRevealed())
            {
                cells[row][col].plantFlag();
                totalFlag++;
                /**
                 *if there is no flag, and cell is not revealed
                 *if there is mine where flag is planted, flagMine will increment itself by 1
                 **/
                if(cells[row][col].isFlag() && cells[row][col].isMine())
                {
                    flagMine++;
                    
                }
            }
            /**
             *if there is a flag, and cell is not revealed
             *it will remove a flag and decrement total flag count;
             **/
            else if(cells[row][col].isFlag() && !cells[row][col].isRevealed())
            {
                /**
                 *if there is no flag, and cell is not revealed
                 *if there is mine where flag is planted, flagMine will decrement itself by 1
                 **/
                if(cells[row][col].isFlag() && cells[row][col].isMine())
                {
                    flagMine--;
                   
                }
                cells[row][col].unplantFlag();
                totalFlag--;               
            }
        
            /** if all the flags that are on a mine is equal to the
             *total mines and there is no other flag else where
             *it will declare the game as won.
             **/
            if(flagMine == numMines && totalFlag == flagMine)
            {    
                for(int i = 0; i < NUM_ROWS; i++)
                {
                    for( int j = 0; j < NUM_COLS; j++)
                    {                   
                        if(cells[i][j].isMine()==false && (cells[row][col].isFlag()==false))
                        {
                            gameWon = true;
                            timer.stopCounting();
                        }
                    }//end of inner for loop
                }//end of outer for loop
            }
        }//end of right clicked pressed
        //if all the cells without mines are revealed, game will be decalered as win
        if(numRevealed == ((NUM_ROWS*NUM_COLS) - numMines))
        {
            gameWon = true;
            timer.stopCounting();
        }
        //If the game is lost, it will print out userfriendly
        //info box stating they lost
        if(gameLost == true)
        {
            Label lost = new Label(350, 25 , "You Lost!");
            lost.setFont("SansSerif Bold", 24);
            lost.setForegroundColor(Canvas.RED);
            lost.setBackgroundColor(Canvas.BLUE);
            lost.setBorderColor(Canvas.BLACK);
            add(lost);
        }
        //If the game is won, it will print out userfriendly
        //info box stating they Won
        if(gameWon == true)
        {
            
            Label winner = new Label(305,25 ,"You Won!");
            winner.setFont("SansSerif Bold", 20);
            winner.setForegroundColor(Canvas.RED);
            winner.setBackgroundColor(Canvas.GREEN);
            winner.setBorderColor(Canvas.WHITE);
            add(winner);
            Label PUBGwinner = new Label(125,25 ,"Victory Royale!");
            PUBGwinner.setFont("SansSerif Bold", 13);
            PUBGwinner.setForegroundColor(Canvas.WHITE);
            PUBGwinner.setBackgroundColor(Canvas.BOOK_BLUE);
            PUBGwinner.setBorderColor(Canvas.WHITE);
            add(PUBGwinner);
            Label fortniteWinner = new Label(525,25 ,"Winner Winner Chicken Dinner!");
            fortniteWinner.setFont("SansSerif Bold", 13);
            fortniteWinner.setForegroundColor(Canvas.YELLOW);
            fortniteWinner.setBackgroundColor(Canvas.BLACK);
            fortniteWinner.setBorderColor(Canvas.WHITE);
            add(fortniteWinner);
        }
    }//end of mouseClicked
    
    /**
     * Respond to key presses. This function will be called each time the user
     * presses a key. The parameter indicates the character the user pressed.
     * The function should update the game state according to what character the
     * user has pressed. 
     * @param c the character that was typed.
     */
    public void keyTyped(char c)
    {
        // User pressed a key, see what they want to do.
        switch (c)
        {
        case 'q': 
        case 'Q': 
            hide(); // user wants to quit
            break;
        case 's': 
        case 'S': 
            save(); // user wants to save
            break;
        case 'R': 
        case 'r': 
            restore(); //user wants to restore
            break;
        default:
            break; // anything else is ignored
        }
    }

    /**
     * checks whether user has won or lost and if not
     *it will print out all of its current data from cell array
     *to the output file which will then be restored if user chooses
     */
    public void save()
    {
        //Declares output file
        Out outFile = new Out("out.txt");
        
        //if user have alredy won, it will print a lable that they can not save current game
        //it will then print won to the output file and returns
        if(gameWon)
        {
            Label winnerError = new Label(240,575,"You can not Save your game because you already Won!");
            winnerError.setFont("SansSerif Bold", 13);
            winnerError.setForegroundColor(Canvas.WHITE);
            winnerError.setBackgroundColor(Canvas.MAROON);
            winnerError.setBorderColor(Canvas.BLACK);
            add(winnerError);
            outFile.println("won");
            return;
        }
        
        //otherwise if user is lost, it will print a lable that they can not save current game
        //it will then print lost to the output file and returns
        else if(gameLost)
        {
            Label LoserError = new Label(240,575 ,"You can not Save your game because you already Lost!");
            LoserError.setFont("SansSerif Bold", 13);
            LoserError.setForegroundColor(Canvas.BLACK);
            LoserError.setBackgroundColor(Canvas.ORANGE);
            LoserError.setBorderColor(Canvas.BLACK);
            add(LoserError);
            outFile.println("lost");
            return;

        }
        /**
         *if user has not won or lost yet,
         *it will store all the current data to the output file
         */
        else
        {
            //Current game status
            outFile.println("GameOn");
            //Number of revealed cells
            outFile.println(numRevealed);
            //elapsed time of the game
            outFile.println(timer.getElapsedSeconds());
            for(int i = 0; i < NUM_ROWS; i++)
            {
                for (int j = 0; j < NUM_COLS; j++)
                {
                    //If there is a flag on the mine
                    if(cells[i][j].isMine() && !cells[i][j].isRevealed() && cells[i][j].isFlag())
                    {
                        outFile.println("hiddenmineFlag" + " " + cells[i][j].getNeighborMineCount());
                    }
                    //if there is flag on the unrevealed cells which are not mine
                    else if(!cells[i][j].isMine() && !cells[i][j].isRevealed() && cells[i][j].isFlag())
                    {
                        outFile.println("onlyFlag" + " " + cells[i][j].getNeighborMineCount());
                    }
                    //if there is a hidden mine on the unrevealed cell
                    else  if (cells[i][j].isMine() && !cells[i][j].isRevealed())
                    {
                        outFile.println("hiddenmine" + " " + cells[i][j].getNeighborMineCount());
                    }
                    //if cell is revealed and there is not a mine
                    else if (cells[i][j].isRevealed() && !cells[i][j].isMine())
                    {
                        outFile.println("revealed" + " " + cells[i][j].getNeighborMineCount());
                    }
                    //other wise all the remaining cells which are not revealed yet
                    else
                    {
                        outFile.println("notRevealed" + " " + cells[i][j].getNeighborMineCount());
                    }               
                }//end of inner for loop                
            }//end of outer for loop
        }
    }// end of save
    
    /**
     * checks whether user has won or lost in the last game  and if not
     *it will print in all of last game's current data from output file
     *to the game and sets the variables according to the last game
     **/
    public void restore()
    {
        //Open the input file
        In inFile = new In("out.txt");
        //Creates variable to store the read In data
        String cellStatus = "";//will read the cell status
        int neighborMineCount = 0;//read in the neighbor mine counts
        String gameStatus = "";//it will read in the game status
        //reads the game Status
        gameStatus = inFile.readString();
        //if the game status is won or lost, it will print a lable that they ca not restore a game and play
        //new game
        if(gameStatus.equals("won") || gameStatus.equals("lost"))
        {
            Label restoreError = new Label(350,25 ,"You can not restore your game because you already Lost or Won in the last Game and still tried to save the game!");
            restoreError.setFont("SansSerif Bold", 10);
            restoreError.setForegroundColor(Canvas.BLACK);
            restoreError.setBackgroundColor(Canvas.ORANGE);
            restoreError.setBorderColor(Canvas.BLACK);
            add(restoreError);
            Label newGame = new Label(240,575,"Please enjoy your new Game!");
            newGame.setFont("SansSerif Bold", 10);
            newGame.setForegroundColor(Canvas.BLACK);
            newGame.setBackgroundColor(Canvas.ORANGE);
            newGame.setBorderColor(Canvas.BLACK);
            add(newGame);
        }

        //If the user is not won or lost in last game and tried to save the game
        //if will restore the previous saved version
        else
        {
            //intialize the numRevealed cells from saved version
            numRevealed = inFile.readInt();
            //resets the timer to zero
            timer.reset(inFile.readInt());
       
            for(int i = 0; i < NUM_ROWS; i++)
            {
                for(int j = 0; j < NUM_COLS; j++)
                {
                    //reads the each cells status
                    cellStatus = inFile.readString();
                    //if the cell has hidden mine, it will plant a mine and not reveal
                    if(cellStatus.equals("hiddenmine"))
                    {
                        cells[i][j].isMine = true;
                        cells[i][j].isRevealed = false;
                    }
                    //if the cell is revealed, it reveal that cell 
                    else if(cellStatus.equals("revealed"))
                    {
                        cells[i][j].isMine = false;
                        cells[i][j].isRevealed = true;
                    }
                    //if the cell is not revealed, it will not plan or reveal that cell
                    else  if(cellStatus.equals("notRevealed"))
                    {
                        cells[i][j].isMine = false;
                        cells[i][j].isRevealed = false;
                    }
                    //if there is a flag and hidden mine on that cell, it
                    //will plan a mine and a flag and not reveal that cell
                    else if(cellStatus.equals("hiddenmineFlag"))
                    {
                        cells[i][j].isFlag = true;
                        cells[i][j].isMine = true;
                        cells[i][j].isRevealed = false;

                    }
                    //if there is only flag, it will plant a flag and not reveal a cell
                    else if(cellStatus.equals("onlyFlag"))
                    {
                        cells[i][j].isFlag = true;
                        cells[i][j].isMine = false;
                        cells[i][j].isRevealed = false;
                    }
                    //reads the neibhor mines  
                    neighborMineCount = inFile.readInt();
                    //sets the neighborMinecounts
                    cells[i][j].setNeighborMineCount(neighborMineCount);
                    //reads the remaining line
                    inFile.readLine();
                }//end of inner for loop
            }//end of outer for loop
        }    
    }//end of restore
    
    /**
     * Paint the background for this window on the canvas. Don't call this
     * directly, it is called by the GUI system automatically. This function
     * should draw something on the canvas, if you like. Or the background can
     * be blank.
     * @param canvas the canvas on which to draw.
     */
    public void repaintWindowBackground(GUI.Canvas canvas)
    {

    }//end of repaintWindowBackground
}//end of Game
