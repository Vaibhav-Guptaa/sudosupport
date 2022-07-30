# sudosupport
Highly optimised code for hardest sudokus using backtracking.

from typing import Tuple, List

def input_sudoku() -> List[List[int]]:

	sudoku= list()
	for _ in range(9):
		row = list(map(int, input().rstrip(" ").split(" ")))
		sudoku.append(row)
	return sudoku

def print_sudoku(sudoku:List[List[int]]) -> None:

	for i in range(9):
		for j in range(9):
			print(sudoku[i][j], end = " ")
		print()

# You have to implement the functions below

def get_block_num(sudoku:List[List[int]], pos:Tuple[int, int]) -> int:

        if(pos[0]%3==0 and pos[1]%3==0):
             return (pos[0]-3)+(pos[1]//3)
        elif(pos[0]%3==0) :
                 return (pos[0])-3+(pos[1]//3+1)
        elif(pos[1]%3==0) :
                return  3*(pos[0]//3)+(pos[1]//3)
        else :
                 return 3*(pos[0]//3)+(pos[1]//3+1)
                
def get_position_inside_block(sudoku:List[List[int]], pos:Tuple[int, int]) -> int:
        
        if(pos[0]%3==0 and pos[1]%3==0):
             return 9
        elif(pos[0]%3==0) :
                 return  6+(pos[1]%3)
        elif(pos[1]%3==0) :
                return  3*(pos[0]%3)
        else :
                return 3*(pos[0]%3-1)+(pos[1]%3)

        
def get_block(sudoku:List[List[int]], x: int) -> List[int]:
        sample = []
        if(x%3!=0) :
         a = x//3
         b= x%3
         for _ in range((a)*3,(a)*3+3) :
            for __ in range((b-1)*3,(b-1)*3+3) :
                           sample.append(sudoku[_][__])
         return sample
        else :
          for _ in range(x-3,x) :
            for __ in range(6,9) :
                     sample.append(sudoku[_][__])
        return sample    

def get_row(sudoku:List[List[int]], i: int)-> List[int]:
        return sudoku[i-1]

def get_column(sudoku:List[List[int]], x: int)-> List[int]:
        sample2 = []
        for _ in range (9) :
            sample2.append(sudoku[_][x-1])
        return sample2


def find_first_unassigned_position(sudoku : List[List[int]]) -> Tuple[int, int]:
        for _ in range(9) :
            for __ in range(9) :
                if(sudoku[_][__] == 0) :
                        return (_+1,__+1)
        return (-1,-1)

def valid_list(lst: List[int])-> bool:
        for _ in range(len(lst)) :
                if(lst.count(lst[_])>1 and lst[_]!= 0) :
                        return False
        return True

def valid_sudoku(sudoku:List[List[int]])-> bool:
        for _ in range (9):
                if(valid_list(sudoku[_])==False) :
                   return False
        for _ in range(9) :
            if(valid_list(get_column(sudoku, _+1))==False) :
                   return False
        for _ in range(9) :
            if(valid_list(get_block(sudoku, _+1))==False) :
                   return False
        
        return True


def get_candidates(sudoku:List[List[int]], pos:Tuple[int, int]) -> List[int]:
        gety = []
        row1 = get_row(sudoku, pos[0])
        col1 = get_column(sudoku, pos[1])
        block1 = get_block(sudoku , get_block_num(sudoku,pos))
        for _ in range (1,10) :
                
                check  = True
                for __ in range(9) :
                        if(row1[__]==_) :
                             check = False
                             break
                        if(col1[__]==_) :
                            check = False
                            break
                        if(block1[__]==_) :
                             check = False
                             break
                if(check ==True) :
                         gety.append(_)
                
        return gety
def make_move(sudoku:List[List[int]], pos:Tuple[int, int], num:int) -> List[List[int]]:
        sudoku[pos[0]-1][pos[1]-1] = num
        return sudoku

def undo_move(sudoku:List[List[int]], pos:Tuple[int, int]):
        sudoku[pos[0]-1][pos[1]-1] = 0
        return sudoku

def sudoku_solver(sudoku: List[List[int]]) -> Tuple[bool, List[List[int]]]:
        ai = find_first_unassigned_position(sudoku)
        if(ai==(-1,-1)) :
                return (True, sudoku)

        favcan = get_candidates(sudoku, ai)
        
        for _ in range(len(favcan)) :
                        
                        make_move(sudoku,ai,favcan[_])
                        if(sudoku_solver(sudoku)[0]) :
                               return(True, sudoku)
                        undo_move(sudoku, ai)
                                
                        
                        
        return (False, sudoku)

def in_lab_component(sudoku: List[List[int]]):
	print("Testcases for In Lab evaluation")
	print("Get Block Number:")
	print(get_block_num(sudoku,(4,4)))
	print(get_block_num(sudoku,(7,2)))
	print(get_block_num(sudoku,(2,6)))
	print("Get Block:")
	print(get_block(sudoku,3))
	print(get_block(sudoku,5))
	print(get_block(sudoku,9))
	print("Get Row:")
	print(get_row(sudoku,3))
	print(get_row(sudoku,5))
	print(get_row(sudoku,9))
