# 16x16sudokuSolver
sudoku puzzle solver: java programming class assignment
import java.io.PrintWriter;
import java.util.ArrayList;
import java.util.Scanner;

public class SudokuSolver {
	//Data fields
	private static final int SIZE = 16;
	private static final char BLANK = '.';
	private static final char[] digits = {'0', '1', '2', '3', '4', '5', '6', '7', '8', '9', 'a', 'b', 'c','d', 'e', 'f'};
	private char[][] board;
	private static final int Grid_Size = 4;
	private int numOfSolutions;
	//Constructor
	
	public SudokuSolver() { board = new char[SIZE][SIZE];} // Default constructor
	
	//methods
	/**
	 * Loads the puzzle from an input file
	 * @param scnr: reads input from input file
	 */
	public void loadData(Scanner scnr) {
		for(int row = 0; row < SIZE; row++) {
			String currentLine = scnr.nextLine().trim();
			for(int col = 0; col < SIZE; col++) {
				board[row][col] = currentLine.charAt(col);
			}
		}
	}
	
	/**
	 * Prints solutions to the output file
	 * @param writer: writes to the output file
	 */
	private void printSolution(PrintWriter writer) {
		writer.printf("Solution: %d\n\n", ++numOfSolutions);
		for(int row = 0; row < SIZE; row++) {
			for(int col = 0; col < SIZE; col++) {
				writer.print(board[row][col]);
			}
			writer.println();
		}
		writer.println();
	}
	/**
	 * Tests whether a digit already appears in a row
	 * @param digit: the  digit to test
	 * @param row: the row to test
	 * @return: {true} if {digit} appears in the row; {false} otherwise
	 */
	private boolean inSameRow(char digit, int row) {
		for(int col = 0; col < SIZE; col++) {
			if(board[row][col] == digit) { return true;}
		}
		return false;
	}
	
	/**
	 * Test whether a digit already exists in a column
	 * @param digit: the digit to test
	 * @param col: the column to test
	 * @return: {true} if {digit} appears in the column; {false} otherwise
	 */
	private boolean inSameCol(char digit, int col) {
		for(int row = 0; row < SIZE; row++) {
			if(board[row][col] == digit) {return true;}
		}
		return false;
	}
	
	/**
	 * Tests whether a digits already exists in a grid
	 * @param digit: digit to test
	 * @param row: row to test
	 * @param col: col to test
	 * @return: {true} if digit is in grid; {false} otherwise
	 */
	private boolean inSameGrid(char digit, int row, int col) {
		for(int i = row / Grid_Size * Grid_Size; i < row / Grid_Size * Grid_Size + Grid_Size; i++) {
			for(int j = col / Grid_Size * Grid_Size; j < col / Grid_Size * Grid_Size + Grid_Size; j++) {
				if(board[i][j] == digit) {return true;}
			}
		}
		return false;
	}
	
	/**
	 * finds the next cell in row-major order
	 * @param row: the row index of the current cell
	 * @param col: the column index of the current cell
	 * @return: an array of the size 2 containing the row index and column index (in that order) of the next cell
	 */
	private int[] nextCell(int row, int col) {
		if(row == SIZE - 1 && col == SIZE - 1) {return new int[] {-1, -1}; }
		if(col == SIZE - 1) {return new int[] {row + 1, 0}; }
		return new int[] {row, col + 1};
	}
	
	private void printAllSolutions(PrintWriter writer, int row, int col) {
		if(row == - 1 || col == - 1) {printSolution(writer);}
		else if(board[row][col] != BLANK) {printAllSolutions(writer, nextCell(row, col)[0], nextCell(row, col)[1]);}
		else {
			for(char digit : digits) {
				if(inSameRow(digit, row)) {continue;}
				if(inSameCol(digit, col)) {continue;}
				if(inSameGrid(digit, row, col)) {continue;}
				board[row][col] = digit;
				printAllSolutions(writer, nextCell(row, col)[0], nextCell(row, col)[1]);
				board[row][col] = BLANK;
			}			
		}
	}
	//wrapper method
	public void printAllSolutions(PrintWriter writer) { printAllSolutions(writer, 0, 0);}
}
