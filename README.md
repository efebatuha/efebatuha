import play
from random import randint
#oyun kuralları
hello = play.new_text(words='Try to get 21 points and beat the comp!', x=0, y=200)

#butonlar
button_add= play.new_box(color='yellow', border_color='grey', border_width=1, x=-130,
y=-160, width=170, height=40)
add_txt= play.new_text(words='Add card', font_size=30,x=-130,y=-160, color='grey')
button_stop= play.new_box(color='red', border_color='grey', border_width=1, x=130,
y=-160, width=190, height=40)
stop_txt=play.new_text(words='Start me VS comp', font_size=30,x=130, y=-160, 
color='grey' )

#kart
card= play.new_image(image='card1.png', size=40, x=-150, y=20)
card.hide()
#sonuçlar
you_score=play.new_text(words='0', x=-100, y=-230 )
comp_score= play.new_text(words='0', x=100, y=-230)
result = play.new_text(words='', x=-200, y=-230) 
steps_txt1= play.new_text(words='Made steps:', x=-20, y=-280, font_size=30)
steps_txt2 = play.new_text(words='0', x=60, y=-280, font_size=30) #oyuncunun hamle sayısı

speed = 1.5 #hamleler arası duraklama

def lose():
    comp_score.color='green'
    you_score.color='red'
    result.color='red'
    result.words='You lose:' #kayıp
def win():
    comp_score.color='red'
    you_score.color='green'
    result.color='green'
    result.words='You win:' #kazanma

def draw():
    result.words='Tie game' #beraberlik

@button_add.when_clicked
async def add_card():
    number = randint(1,11)
    card.image = 'card' + str(number) + '.png'
    card.show()
    you_score.words = int(you_score.words) + number
    steps_txt2.words =int(steps_txt2.words) + 1 
    await play.timer(speed)
    card.hide()
    if int(you_score.words) > 21 : #kayıp
        lose()
@button_stop.when_clicked
async def comp_play():
    card.x = 150
    steps = int(steps_txt2.words)
    for i in range(steps):
        number =randint(1,11)
        card.image = 'card' + str(number) + '.png'
        card.show()
        comp_score.words = int(comp_score.words) + number
        await play.timer(speed)
        card.hide()
    #kazanma ve kaybetme
    if int(comp_score.words) > 21 or int(comp_score.words) < int(you_score.words):
        win()   #kullanıcı kazanır
    elif int(comp_score.words) > int(you_score.words):
        lose()  #kullanıcı kaybeder
    else:
        draw()





play.start_program()
