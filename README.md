# Expression
Basically converting a string to mathematical expression¶
import nltk #natural language toolkit for tokenizing list
from word2number import w2n #for convering words to numbers
​
#this is to detect the keywords in the input to recognize which operation to perform
operators={"add","sum","multiply","multiplied","subtract","divide","divided","minus","plus","product","+","-","*","/","difference"}
​
#this is to recognize operators in the input since input is in string format
op={"+","-","/","*"}
​
#this is to recognise connectors that we use in our input
connectors={"from","and",","}
​
#these are the words that we don't need in our actual expression
remove={"of","by","to","is","between"}
​
# this is to map string operators to their corresponding mathematical opertators..
mapOperator={"add":'+','sum':'+',"plus":'+',"multiply":'*',"multiplied":'*',"product":'*',"subtract":'-',
            "divide":'/',"divided":'/',"minus":'-',"+":'+',"-":'-',"*":'*',"/":'/',"difference":'-'}
​
# this is to recognize the word numbers
nums= { "zero","one","two","three","four", "five", "six","seven","eight","nine","ten",
       "eleven","twelve","thirteen", "fourteen", "fifteen","sixteen","seventeen", "eighteen",
       "nineteen","twenty","thirty","forty", "fifty","sixty","seventy","eighty","ninety","hundred","thousand",
       "million", "billion", "trillion"}
​
# this is used as multiplier if we need one
multipliers= {"hundred","thousand","million", "billion", "trillion"}
#this function process the input string and remove unneccessary words and 
#convert string operators to mathematical one and returns the final lis
​
def process_input(inp):
    li=nltk.word_tokenize(inp)
    print("Raw list: ",li)
    wrd_exp=[]
    for i in range(len(li)):
        li[i]= li[i].lower() #input keywords are coverted to lowercase for matching
        if li[i] not in remove: #appending ony those words which we want in our expression
            if li[i] in operators: #mapping the string operators to corresponding mathematical one
                cur_op= mapOperator[li[i]]
                if len(wrd_exp) != 0 and wrd_exp[-1] not in op:
                    wrd_exp.append(cur_op)
            elif li[i] in connectors: #appending the connectors as well..they help to distinguish between two different numbers
                wrd_exp.append(cur_op)
            elif (li[i] in nums) or li[i].replace('.','',1).isdigit(): #appending the numbers (words, integers and floating points as well)
                wrd_exp.append(li[i])
    print("Mathematical Expression: ",wrd_exp)
    return wrd_exp
#this takes input raw list of numbers, operators and connectors and returns the final expression list
def maths_expression(args):
    express=[]
    number=''
    for i in range(len(args)+1):
        if i< len(args) and args[i] in nums: # seperating different word numbers
            if number != '':
                number+=' '+ args[i]
            else:
                number+=args[i]
        else : #appending the the numberic form of numbers that i have seperated( words nmbers are converted to numeric form as well)
            if number !='' and number.split()[0] in multipliers: #when we need multipliers
                number='one '+number
            if number != '':
                express.append(str(w2n.word_to_num(number)))   #converts word numbers to numeric form then again into string format
            if i<len(args) and args[i] not in nums: #finally generating the expression list
                express.append(args[i])
            number=''
    print("Final Expression list:",express)
    return express
# this function simply takes strings in a list to concatenate each element and returns the final single string
def list_to_str(express_list):
    exp=''
    for e in express_list:
        exp= exp+e;
    return exp 
# this function combine all the operations..
def final():
    inp= input("Enter the input:") #taking input
    raw_list= process_input(inp) #removing unneccessary words
    expression_list= maths_expression(raw_list) #converting raw list into meaningful expression list 
    expression= list_to_str(expression_list) # converts expression list to sigle string
    print("Final Expression:",expression)
    ans= eval(expression) #eval() predefined function- takes a expression string and evaluates it to produce final o/p using BODMAS
    return ans
    
print("Ans:",final())
Enter the input:1 + 2 / 3 multiply four
Raw list:  ['1', '+', '2', '/', '3', 'multiply', 'four']
Mathematical Expression:  ['1', '+', '2', '/', '3', '*', 'four']
Final Expression list: ['1', '+', '2', '/', '3', '*', '4']
Final Expression: 1+2/3*4
Ans: 3.6666666666666665
