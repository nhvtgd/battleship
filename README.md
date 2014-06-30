battleship
==========
__author__ = 'vnguyen'
import requests
postURL = "https://student.people.co/api/challenge/battleship/0116d9086630/boards/"
getBoard = "https://student.people.co/api/challenge/battleship/0116d9086630/boards/"
deleteBoard = "https://student.people.co/api/challenge/battleship/0116d9086630/boards/"
column = ['A','B','C','D','E','F','G','H','I','J']
row = range(1,11)
translate = {'false': 'False', 'true': 'True', 'null': 'None'}
totalHit = 5+ 4 + 3 + 3 + 2
currentHit = 0
def shotLocation(boardId,location):
    url = postURL + boardId + "/" + location
    print url
    result = requests.post(url)
    if result.text:
        return eval(result.text, translate)
    else:
        return None

def getBoardDic(boardID):
    url = getBoard + boardID
    result = requests.get(url)
    if result.text:
        return eval(result.text, translate)
    else:
        return None

def deleteBoardDic(boardID, location = None):
    url = deleteBoard + boardID
    if location == None:
        result = requests.delete(url)
    else:
        result = requests.delete(url+ "/" + location)
    return result.ok

def prepopulateBoard():
    dic = {}
    for i in column:
        for j in row:
            dic[i+str(j)] = False
    return dic

def isHit(postResponse):
    return postResponse["is_hit"]

def basicVersion(board):
    global currentHit
    for i in column:
        for j in row:
            hit = shotLocation(board, i + str(j))
            if hit["is_hit"]:
                currentHit += 1
            if isDone():
                currentHit = 0
                return getBoardDic(board)



def isDone():
    return currentHit == totalHit

#deleteBoardDic("test_board_1")
for i in range(1,6):
    basicVersion("live_board_"+ str(i))
