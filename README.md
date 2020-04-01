файлы /
config_all_ru /
ошибки /
# Байт-скомпилированные / оптимизированные / DLL файлы
__pycache__ /
* .py [ cod ]
* $ py.class
default_language_version :
    python : python3.7
репо :
-    репо : https://gitlab.com/pycqa/flake8
    rev : ' '
    крючки :
    -    id : flake8
Лицензия MIT

Copyright (c) 2019 Ava.City

Таким образом, разрешение предоставляется бесплатно любому лицу, получающему копию
этого программного обеспечения и связанных с ним файлов документации («Программное обеспечение»), чтобы иметь дело
в Программном обеспечении без ограничений, в том числе без ограничения прав
использовать, копировать, изменять, объединять, публиковать, распространять, сублицензировать и / или продавать
копии Программного обеспечения, а также разрешать лицам, которым
предоставлены для этого при соблюдении следующих условий:

Вышеуказанное уведомление об авторских правах и это уведомление о разрешении должны быть включены во все
копии или существенные части Программного обеспечения.

ПРОГРАММНОЕ ОБЕСПЕЧЕНИЕ ПРЕДОСТАВЛЯЕТСЯ «КАК ЕСТЬ», БЕЗ КАКИХ-ЛИБО ГАРАНТИЙ, ЯВНЫХ ИЛИ ЯВНЫХ
ПОДРАЗУМЕВАЕМЫЙ, ВКЛЮЧАЯ, НО НЕ ОГРАНИЧИВАЯСЬ ​​ГАРАНТИЙ ТОВАРОВ
ФИТНЕС ДЛЯ ОСОБЫХ ЦЕЛЕЙ И НЕФРИФРИНГЕНТОВ. НИ В КОЕМ СЛУЧАЕ
АВТОРЫ ИЛИ АВТОРСКИЕ ПРАВ ЧЕЛОВЕКА НЕСУТ ОТВЕТСТВЕННОСТЬ ЗА ЛЮБОЙ ТРЕБОВАНИЕ, УЩЕРБ ИЛИ ДРУГОЕ
ОТВЕТСТВЕННОСТЬ, ДЕЙСТВУЮЩАЯ ЛИ В ДЕЙСТВИИ С ДОГОВОРОМ, ИСПЫТАНИЕМ ИЛИ ИНЫМ ОБРАЗОМ, возникающим из
ИЛИ ИЛИ В СВЯЗИ С ПРОГРАММНЫМ ОБЕСПЕЧЕНИЕМ ИЛИ ИСПОЛЬЗОВАНИЕМ ИЛИ ДРУГИМИ СДЕЛКАМИ
ПРОГРАММНОЕ ОБЕСПЕЧЕНИЕ.
 ведение журнала импорта
импорт  бинасксии
 время импорта
из  цепочки битов  импортировать  BitArray , ConstBitStream
 протокол импорта
импорт  общего
импорт  конст


Класс  Client ():
    def  __init__ ( self , server ):
        сам . сервер  =  сервер
        сам . UID  =  Нет
        сам . encrypted  =  False
        сам . сжатый  =  ложный
        сам . контрольная сумма  =  ложь
        сам . комната  =  ""
        сам . позиция  = ( 0 , 0 )
        сам . размерность  =  4
        сам . состояние  =  0
        сам . action_tag  =  ""

     дескриптор def ( self , connection , address ):
        сам . адрес  =  адрес
        сам . соединение  =  соединение
        каротаж . отладка ( f "Соединение с { address [ 0 ] } " )
        buffer  =  ConstBitStream ()
        пока  верно :
            попробуй :
                данные  =  соединение . recv ( 1024 )
            кроме  OSError :
                перерыв
            если  не  данные :
                перерыв
            данные  =  буфер  +  ConstBitStream ( данные )
            buffer  =  ConstBitStream ()
            если  данные . hex  ==  "3c706f6c6963792d66696c652d726571756573742f3e00" :
                send_data  =  BitArray ( Const . XML )
                send_data . добавить ( "0x00" )
                подключение . отправить ( send_data . байты )
                Продолжать
            в то время как  len ( data ) -  данные . pos  >  32 :
                длина  =  данные . читать ( 32 ). ИНТ
                if ( len ( data ) -  data . pos ) /  8  <  длина :
                    данные . pos  =  0
                    перерыв
                final_data  =  протокол . processFrame ( data . read ( длина  *  8 ),
                                                   Правда )
                если  final_data :
                    попробуй :
                        сам . сервер . process_data ( final_data , самостоятельно )
                    кроме  исключения :
                        каротаж . исключение ( "Ошибка при обработке данных" )
            если  лен ( данные ) -  данные . pos  >  0 :
                буфер  =  данные . читать ( len ( data ) -  data . pos )
        сам . _close_connection ()

    def  send ( self , msg , type_ = 34 ):
        data  =  BitArray ( f "int: 8 = { type_ } " )
        данные . append ( протокол . encodeArray ( msg ))
        данные . insert ( self . _make_header ( data ), 0 )
        попробуй :
            сам . подключение . отправить ( data . bytes )
        кроме ( BrokenPipeError , OSError ):
            сам . подключение . закрыть ()

    def  _make_header ( self , msg ):
        buf  =  BitArray ()
        header_length  =  1
        если  сам . контрольная сумма :
            header_length  + =  4
        Buf . append ( f "int: 32 = { len ( msg . hex ) // 2 + header_length } " )
        Buf . append ( f "int: 8 = { self . _header_to_byte () } " )
        если  сам . контрольная сумма :
            Buf . append ( f "uint: 32 = { binascii . crc32 ( msg . bytes ) % ( 1 << 32 ) } " )
        возврат  буф

    def  _header_to_byte ( self ):
        маска  =  0
        если  сам . зашифровано :
            маска | = ( 1  <<  1 )
        если  сам . сжатый :
            маска | = ( 1  <<  2 )
        если  сам . контрольная сумма :
            маска | = ( 1  <<  3 )
         маска возврата

    def  _close_connection ( self ):
        сам . подключение . закрыть ()
        каротаж . отладка ( f "Соединение закрыто с { self . address [ 0 ] } " )
        если  сам . UID :
            если  сам . комната :
                префикс  =  общий . get_prefix ( самостоятельно . номер )
                для  клиента  в  себе . сервер . онлайн . copy ():
                    если  клиент . комната  ! =  я . комната :
                        Продолжать
                    клиент . отправить ([ префикс + ".r.lv" , { "uid" : self . uid }])
                    клиент . отправить ([ self . room , self . uid ], type_ = 17 )
            если  сам . Uid  в  себе . сервер . inv :
                сам . сервер . и [ самостоятельно . UID ]. истекает  =  время . время () + 30
            сам . сервер . онлайн . удалить ( самостоятельно )
        Del  Self
 ведение журнала импорта


def  get_prefix ( location ):
    префикс  =  местоположение . split ( "_" ) [ 0 ]
    если  префикс  в [ "cafe" , "club" , "street" , "yard" , "skiResort" , "publicBeach" ,
                  "кутюрье" , "бальный зал" , "парк" , "каньон" , "салон" ,
                  "фотосалон" , "weddingBeach" , "iceRink" , "подиум" , "сад" ,
                  "avaCitySchool" , "islBeach" , "avaBirthday2016Beach" ]:
        возврат  "о"
     префикс  elif ==  "дом" :
        вернуть  "ч"
     префикс  elif ==  "работа" :
        возврат  "w"
    еще :
        каротаж . предупреждение ( f "Префикс местоположения { префикс } не найден" )
        возврат  "о"
класс  WrongRoom ( исключение ):
    проходят
<? xml version = " 1.0 " encoding = " UTF-8 " ?>
< словарь >
    < locale  name = " ru " >
        < message  key = " showModeratorActions " > Действия модератора </ message >
        < message  key = " moderatorRatingsLabel " > Рейтинг модераторов </ message >
        < message  key = " checkSocialAPI " > Проверка социального API </ message >
        < message  key = " clothesPostEditor " > Редактор одежды для постов </ message >
        < message  key = " findUserButtonLabel " > Найти пользователя </ message >
        < message  key = " reconnectButtonLabel " > Перезайти как: </ message >
    </ locale >
</ словарь >
files/swfobject.js
 ведение журнала импорта


 Инвентаризация класса ():
    def  __init__ ( self , server , uid ):
        сам . сервер  =  сервер
        сам . UID  =  UID
        сам . истекает  =  нет
        сам . _get_inventory ()

    def  get ( self ):
        вернуть  себя . фактура

    def  add_item ( self , name , type_ , amount = 1 ):
        если  "_"  в  имени :
            TID , IID  =  имя . разделить ( "_" )
        еще :
            TID  =  имя
            iid  =  ""
        пункт  =  себя . сервер . Redis . lrange ( f "uid: { self . uid } : items: { name } " ,
                                        0 , - 1 )
        если  элемент :
            если  type_  ==  "cls" :
                каротаж . ошибка ( «Не может быть более одной ткани» )
                возвращение
            сам . сервер . Redis . lset ( f "uid: { self . uid } : items: { name } " , 1 ,
                                   int ( элемент [ 1 ]) + сумма )
            для  ТМП  в  себе . inv [ "c" ] [ type_ ] [ "it" ]:
                if  tmp [ "tid" ] ==  tid  и  tmp [ "iid" ] ==  iid :
                    tmp [ "c" ] =  int ( item [ 1 ]) + сумма
                    перерыв
        еще :
            сам . сервер . Redis . sadd ( f "uid: { self . uid } : items" , name )
            сам . сервер . Redis . rpush ( f "uid: { self . uid } : items: { name } " , type_ ,
                                    сумма )
            type_items  =  self . inv [ "c" ] [ type_ ] [ "it" ]
            type_items . append ({ "c" : количество , "tid" : tid , "iid" : iid })

    def  take_item ( self , item , amount = 1 ):
        предметы  =  себя . сервер . Redis . smembers ( f "uid: { self . uid } : items" )
        если  товар  не  в  товарах :
            вернуть  Ложь
        tmp  =  self . сервер . Redis . lrange ( f "uid: { self . uid } : items: { item } " , 0 , - 1 )
        type_  =  tmp [ 0 ]
        иметь  =  int ( tmp [ 1 ])
        Del  Tmp
        если  есть  <  количество :
            вернуть  Ложь
        type_items  =  self . inv [ "c" ] [ type_ ] [ "it" ]
        если  есть  >  сумма :
            сам . сервер . Redis . lset ( f "uid: { self . uid } : items: { item } " , 1 ,
                                   есть  -  сумма )
            для  tmp  в  type_items :
                if  tmp [ "tid" ] ==  item :
                    tmp [ "c" ] =  иметь  -  количество
                    перерыв
        еще :
            сам . сервер . Redis . удалить ( f "uid: { self . uid } : items: { item } " )
            сам . сервер . Redis . srem ( f "uid: { self . uid } : items" , item )
            для  tmp  в  type_items :
                if  tmp [ "tid" ] ==  item :
                    type_items . удалить ( tmp )
                    перерыв
        верните  True

    def  get_item ( self , item ):
        предметы  =  себя . сервер . Redis . smembers ( f "uid: { self . uid } : items" )
        если  товар  не  в  товарах :
            вернуть  0
        have  =  int ( self . server . redis . lindex ( f "uid: { self . uid } : items: { item } " , 1 ))
        возврат  есть

    Защиту  change_wearing ( самостоятельная , ткань , носить ):
        если  не  я . сервер . Redis . lindex ( f "uid: { self . uid } : items: { fabric } " , 0 ):
            not_found  =  True
        еще :
            not_found  =  False
        если  "_"  в  ткани :
            TID , IID  =  ткань . разделить ( "_" )
        еще :
            тид  =  ткань
            iid  =  ""
        type_items  =  self . inv [ "c" ] [ "cls" ] [ "it" ]
        если  носить :
            если  not_found :
                каротаж . ошибка ( f "Ткань { ткань } не найдена для { self . uid } " )
                возвращение
            если  "_"  в  ткани :
                имя  =  ткань . split ( "_" ) [ 0 ]
            еще :
                имя  =  ткань
            сам . _check_conflicts ( имя )
            для  элемента  в  type_items :
                if  item [ "tid" ] ==  tid  и  item [ "iid" ] ==  iid :
                    type_items . удалить ( элемент )
                    перерыв
            сам . сервер . Redis . sadd ( f "uid: { self . uid } : носить" , ткань )
        еще :
            Weared  =  себя . сервер . Redis . запахи ( f "uid: { self . uid } : носить" )
            если  ткань  не  в  изношенном :
                каротаж . ошибка ( f "Ткань { ткань } не была одета для { self . uid } " )
                возвращение
            если  не  not_found :
                type_items . append ({ "c" : 1 , "iid" : iid , "tid" : tid })
            сам . сервер . Redis . srem ( f "uid: { self . uid } : носить" , ткань )

    def  __get_expire ( self ):
        вернуть  себя . __expire

    def  __set_expire ( self , value ):
        сам . __expire  =  значение

    expire  =  свойство ( __get_expire , __set_expire )

    def  _get_inventory ( self ):
        сам . inv  = { "c" : { "frn" : { "id" : "frn" , "it" : []},
                          "act" : { "id" : "act" , "it" : []},
                          "gm" : { "id" : "gm" , "it" : []},
                          "lt" : { "id" : "lt" , "it" : []},
                          "cls" : { "id" : "cls" , "it" : []}}}
        носить  =  себя . сервер . Redis . запахи ( f "uid: { self . uid } : носить" )
        ключи  = []
        труба  =  себя . сервер . Redis . трубопровод ()
        за  предмет  в  себе . сервер . Redis . smembers ( f "uid: { self . uid } : items" ):
            если  элемент  в  ношении :
                Продолжать
            труба . lrange ( f "uid: { self . uid } : items: { item } " , 0 , - 1 )
            ключи . добавить ( элемент )
        предметы  =  труба . выполнить ()
        для  меня  в  диапазоне ( лен ( ключи )):
            имя  =  ключи [ я ]
            item  =  items [ i ]
            если  "_"  в  имени :
                сам . inv [ "c" ] [ item [ 0 ]] [ "it" ]. append ({ "c" : int ( item [ 1 ]),
                                                     "IID" : имя . split ( "_" ) [ 1 ],
                                                     "тид" : имя . split ( "_" ) [ 0 ]}
                                                    )
            еще :
                сам . inv [ "c" ] [ item [ 0 ]] [ "it" ]. append ({ "c" : int ( item [ 1 ]),
                                                     "iid" : "" ,
                                                     "тид" : имя })

    Защиту  _check_conflicts ( самостоятельно , ткань ):
        пол  =  "мальчик"  if ( self . server . get_appearance ( self . uid )) [ "g" ] ==  1 \
                          еще  "девушка"
        категория  =  я . сервер . модули [ "а" ]. get_category ( ткань , пол )
        если  не  категория :
            каротаж . ошибка ( «Категория не найдена» )
            возвращение
        Weared  =  себя . сервер . Redis . запахи ( f "uid: { self . uid } : носить" )
        для  weared_cloth  в  weared :
            если  сам . _has_conflict ( weared_cloth , категория , пол ):
                сам . переодевание ( weared_cloth , False )

    def  _has_conflict ( личность , одежда , категория , пол ):
        get_category  =  self . сервер . модули [ "а" ]. get_category
        fabric_category  =  get_category ( ткань , пол )
        если  fabric_category  ==  категория :
            верните  True
        для  конфликта  в  себе . сервер . конфликты :
            if ( конфликт [ 0 ] ==  категория  и
                конфликт [ 1 ] ==  ткань_категория ) или \
               ( конфликт [ 1 ] ==  категория  и
               конфликт [ 0 ] ==  ткань_категория ):
                верните  True
        вернуть  Ложь
 ведение журнала импорта
из  модулей . base_module  импорта  модуля
из  модулей . импорт местоположения  refresh_avatar 
из  инвентаря  импорт  инвентарь
импорт  конст

class_name  =  "Аватар"


Класс  Аватар ( Модуль ):
    префикс  =  "а"

    def  __init__ ( self , server ):
        сам . сервер  =  сервер
        сам . commands  = { "apprnc" : self . внешность , "clths" : сам . одежда }
        сам . список одежды  =  сервер . синтаксический анализатор . parse_clothes ()
        сам . устанавливает  =  сервер . синтаксический анализатор . parse_cloth_sets ()

    Защиту  внешний вид ( самостоятельно , сообщ , клиент ):
        подкоманда  =  msg [ 1 ]. сплит ( "." ) [ 2 ]
        если  подкоманда  ==  "rnn" :
            if  len ( msg [ 2 ] [ "unm" ]) >  const . MAX_NAME_LEN :
                возвращение
            сам . сервер . Redis . lset ( f "uid: { client . uid } : внешний вид" ,
                                   0 , сообщение [ 2 ] [ "unm" ])
            user_data  =  self . сервер . get_user_data ( client . uid )
            клиент . отправить ([ "a.apprnc.rnn" , { "res" : { "slvr" : user_data [ "slvr" ],
                                                  "enrg" : user_data [ "enrg" ],
                                                  "emd" : user_data [ "emd" ],
                                                  "gld" : user_data [ "gld" ]},
                                          "unm" : msg [ 2 ] [ "unm" ]}])
         подкоманда  elif ==  "сохранить" :
            apprnc  =  msg [ 2 ] [ "apprnc" ]
            current_apprnc  =  self . сервер . get_appearance ( client . uid )
            если  не  current_apprnc :
                сам . update_appearance ( apprnc , клиент )
                сам . сервер . inv [ клиент . uid ] =  Инвентарь ( сам . сервер ,
                                                        клиент . UID )
                if  apprnc [ "g" ] ==  1 :
                    weared  = [ "boyShoes8" , "boyPants10" , "boyShirt14" ]
                    available  = [ "boyUnderdress1" ]
                еще :
                    weared  = [ "girlShoes14" , "girlPants9" , "girlShirt12" ]
                    available  = [ "girlUnderdress1" , "girlUnderdress2" ]
                для  товара  в  наличии + доступно :
                    сам . сервер . inv [ клиент . UID ]. add_item ( item , "cls" )
                для  предмета  в  изношенном :
                    сам . сервер . inv [ клиент . UID ]. change_wearing ( item , True )
                для  предмета  в  постоян . room_items :
                    сам . сервер . модули [ "фрн" ]. add_item ( предмет , "гостиная" ,
                                                        клиент . UID )
                    для  я  в  диапазоне ( 1 , 6 ):
                        сам . сервер . модули [ "фрн" ]. add_item ( item , str ( i ),
                                                            клиент . UID )
            еще :
                if  apprnc [ "g" ] ! =  current_apprnc [ "g" ]:
                    каротаж . ошибка ( "пол не совпадает!" )
                    возвращение
                сам . update_appearance ( apprnc , клиент )
            клиент . отправить ([ "a.apprnc.save" ,
                         { "apprnc" : самостоятельно . сервер . get_appearance ( client . uid )}])

     одежда def ( self , msg , client ):
        подкоманда  =  msg [ 1 ]. сплит ( "." ) [ 2 ]
        если  подкоманда  ==  "носить" :
            сам . wear_cloth ( msg [ 2 ] [ "clths" ], клиент )
         подкоманда  elif ==  "купить" :
            сам . buy_cloth ( msg [ 2 ] [ "tpid" ], клиент )
         подкоманда  elif ==  "bst" :
            сам . buy_clothes_suit ( msg [ 2 ] [ "tpid" ], клиент )
        elif  subcommand  ==  "bcc" :
            сам . buy_colored_clothes ( msg [ 2 ] [ "clths" ], клиент )
        еще :
            каротаж . предупреждение ( f "Команда { msg [ 1 ] } не найдена" )

    def  wear_cloth ( self , clths , client ):
        предметы  =  себя . сервер . Redis . smembers ( f "uid: { client . uid } : items" )
        для  ткани  в  клетках :
            если  ткань [ "clid" ]:
                tmp  =  f " { ткань [ 'tpid' ] } _ { ткань [ 'clid' ] } "
            еще :
                tmp  =  ткань [ "tpid" ]
            если  тмп  не  в  пунктах :
                каротаж . ошибка ( f " { tmp } не найден для { client . uid } " )
                возвращение
        носить  =  себя . сервер . Redis . smembers ( f "uid: { client . uid } : носить" )
        за  ткань  в  ношении :
            сам . сервер . inv [ клиент . UID ]. переодевание ( ткань , ложь )
        для  ткани  в  клетках :
            если  ткань [ "clid" ]:
                tmp  =  f " { ткань [ 'tpid' ] } _ { ткань [ 'clid' ] } "
            еще :
                tmp  =  ткань [ "tpid" ]
            сам . сервер . inv [ клиент . UID ]. change_wearing ( tmp , True )
        inv  =  self . сервер . inv [ клиент . UID ]. получить ()
        clths  =  self . сервер . get_clothes ( client . uid , type_ = 2 )
        ccltn  =  self . сервер . get_clothes ( client . uid , type_ = 3 )
        клиент . отправить ([ "a.clths.wear" , { "inv" : inv , "clths" : clths ,
                                      "ccltn" : ccltn , "cn" : "" ,
                                      "ctp" : "casual" }])
        refresh_avatar ( клиент , самостоятельно . сервер )

    Защиту  buy_cloth ( самостоятельно , ткань , клиент ):
        предметы  =  себя . сервер . Redis . smembers ( f "uid: { client . uid } : items" )
        если  ткань  в  предметах :
            возвращение
        если  сам . сервер . get_appearance ( client . uid ) [ "g" ] ==  1 :
            пол  =  "мальчик"
        еще :
            пол  =  "девушка"
        категория  =  я . get_category ( ткань , пол )
        если  не  категория :
            возвращение
        attrs  =  self . список одежды [ пол ] [ категория ] [ ткань ]
        user_data  =  self . сервер . get_user_data ( client . uid )
        if  user_data [ "gld" ] <  attrs [ "gold" ] или \
           user_data [ "slvr" ] <  attrs [ "silver" ]:
            возвращение
        сам . сервер . Redis . set ( f "uid: { client . uid } : gld" ,
                              user_data [ "gld" ] -  attrs [ "gold" ])
        сам . сервер . Redis . set ( f "uid: { client . uid } : slvr" ,
                              user_data [ "slvr" ] -  attrs [ "silver" ])
        сам . сервер . Redis . set ( f "uid: { client . uid } : crt" ,
                              user_data [ "crt" ] +  attrs [ "rating" ])
        сам . сервер . inv [ клиент . UID ]. add_item ( ткань , "cls" )
        сам . сервер . inv [ клиент . UID ]. смена одежды ( ткань , правда )
        inv  =  self . сервер . inv [ клиент . UID ]. получить ()
        clths  =  self . сервер . get_clothes ( client . uid , type_ = 2 )
        ccltn  =  self . сервер . get_clothes ( client . uid , type_ = 1 )
        ccltn  =  ccltn [ "ccltns" ] [ "casual" ]
        user_data  =  self . сервер . get_user_data ( client . uid )
        клиент . отправить ([ "a.clths.buy" , { "inv" : inv ,
                                     "res" : { "slvr" : user_data [ "slvr" ],
                                             "enrg" : user_data [ "enrg" ],
                                             "emd" : user_data [ "emd" ],
                                             "gld" : user_data [ "gld" ]},
                                     "clths" : clths , "ccltn" : ccltn ,
                                     "crt" : user_data [ "crt" ]}])

    def  buy_clothes_suit ( self , tpid , client ):
        если  сам . сервер . get_appearance ( client . uid ) [ "g" ] ==  1 :
            пол  =  "мальчик"
        еще :
            пол  =  "девушка"
        если  тпид  не  в  себе . устанавливает [ пол ]:
            каротаж . ошибка ( f "Set { tpid } not found" )
            возвращение
        золото  =  0
        серебро  =  0
        рейтинг  =  0
        предметы  =  себя . сервер . Redis . smembers ( f "uid: { client . uid } : items" )
        to_buy  = []
        для  ткани  в  себе . устанавливает [ пол ] [ тпид ]:
            если  ":"  в  ткани :
                ткань  =  ткань . заменить ( ":" , "_" )
            если  ткань  в  предметах :
                Продолжать
            категория  =  я . get_category ( ткань , пол )
            если  не  категория :
                Продолжать
            attrs  =  self . список одежды [ пол ] [ категория ] [ ткань ]
            gold  + =  attrs [ "gold" ]
            silver  + =  attrs [ "silver" ]
            рейтинг  + =  атрибуты [ "рейтинг" ]
            to_buy . добавить ( ткань )
        user_data  =  self . сервер . get_user_data ( client . uid )
        if  user_data [ "gld" ] <  gold  или  user_data [ "slvr" ] <  silver :
            возвращение
        сам . сервер . Redis . set ( f "uid: { client . uid } : gld" ,
                              user_data [ "gld" ] -  золото )
        сам . сервер . Redis . set ( f "uid: { client . uid } : slvr" ,
                              user_data [ "slvr" ] -  серебро )
        сам . сервер . Redis . set ( f "uid: { client . uid } : crt" ,
                              user_data [ "crt" ] +  рейтинг )
        для  одежды  в  to_buy :
            сам . сервер . inv [ клиент . UID ]. add_item ( ткань , "cls" )
            сам . сервер . inv [ клиент . UID ]. смена одежды ( ткань , правда )
        inv  =  self . сервер . inv [ клиент . UID ]. получить ()
        clths  =  self . сервер . get_clothes ( client . uid , type_ = 2 )
        ccltn  =  self . сервер . get_clothes ( client . uid , type_ = 1 )
        ccltn  =  ccltn [ "ccltns" ] [ "casual" ]
        user_data  =  self . сервер . get_user_data ( client . uid )
        клиент . отправить ([ "a.clths.buy" , { "inv" : inv ,
                                     "res" : { "slvr" : user_data [ "slvr" ],
                                             "enrg" : user_data [ "enrg" ],
                                             "emd" : user_data [ "emd" ],
                                             "gld" : user_data [ "gld" ]},
                                     "clths" : clths , "ccltn" : ccltn ,
                                     "crt" : user_data [ "crt" ]}])

    def  buy_colored_clothes ( личность , одежда , клиент ):
        предметы  =  себя . сервер . Redis . smembers ( f "uid: { client . uid } : items" )
        если  сам . сервер . get_appearance ( client . uid ) [ "g" ] ==  1 :
            пол  =  "мальчик"
        еще :
            пол  =  "девушка"
        золото  =  0
        серебро  =  0
        рейтинг  =  0
        to_buy  = []
        за  предмет  в  одежде :
            ткань  =  предмет [ "тпид" ]
            clid  =  item [ "clid" ]
            если  f " { ткань } _ { clid } "  в  элементах  или  ткань  в  элементах :
                Продолжать
            для  категории  в  себе . список одежды [ пол ]:
                за  предмет  в  себе . список одежды [ пол ] [ категория ]:
                    если  предмет  ==  ткань :
                        tmp  =  self . список одежды [ пол ] [ категория ] [ предмет ]
                        gold  + =  tmp [ "gold" ]
                        silver  + =  tmp [ "silver" ]
                        рейтинг  + =  tmp [ "рейтинг" ]
                        если  clid :
                            to_buy . Append ( е " { ткань } _ { CLID } " )
                        еще :
                            to_buy . добавить ( ткань )
                        перерыв
        user_data  =  self . сервер . get_user_data ( client . uid )
        если  не  to_buy  или  user_data [ "gld" ] <  gold  или  user_data [ "slvr" ] <  silver :
            возвращение
        труба  =  себя . сервер . Redis . трубопровод ()
        труба . set ( f "uid: { client . uid } : gld" , user_data [ "gld" ] -  gold )
        труба . set ( f "uid: { client . uid } : slvr" , user_data [ "slvr" ] -  серебро )
        труба . set ( f "uid: { client . uid } : crt" , user_data [ "crt" ] +  рейтинг )
        труба . выполнить ()
        для  одежды  в  to_buy :
            сам . сервер . inv [ клиент . UID ]. add_item ( ткань , "cls" )
            сам . сервер . inv [ клиент . UID ]. смена одежды ( ткань , правда )
        user_data  =  self . сервер . get_user_data ( client . uid )
        inv  =  self . сервер . inv [ клиент . UID ]. получить ()
        clths  =  self . сервер . get_clothes ( client . uid , type_ = 2 )
        ccltn  =  self . сервер . get_clothes ( client . uid , type_ = 1 )
        ccltn  =  ccltn [ "ccltns" ] [ "casual" ]
        клиент . отправить ([ "a.clths.bcc" , { "inv" : inv ,
                                     "res" : { "gld" : user_data [ "gld" ],
                                             "slvr" : user_data [ "slvr" ],
                                             "emd" : user_data [ "emd" ],
                                             "enrg" : user_data [ "enrg" ]},
                                     "clths" : clths , "ccltn" : ccltn ,
                                     "crt" : user_data [ "crt" ]}])

    def  update_appearance ( self , apprnc , client ):
        сам . сервер . Redis . удалить ( f "uid: { client . uid } : внешний вид" )
        сам . сервер . Redis . rpush ( f "uid: { client . uid } : внешний вид" , apprnc [ "n" ],
                                apprnc [ "nct" ], apprnc [ "g" ], apprnc [ "sc" ],
                                apprnc [ "ht" ], apprnc [ "hc" ], apprnc [ " brt " ],
                                apprnc [ "brc" ], apprnc [ "et" ], apprnc [ "ec" ],
                                apprnc [ "fft" ], apprnc [ "fat" ], apprnc [ "fac" ],
                                apprnc [ "ss" ], apprnc [ "ssc" ], apprnc [ "mt" ],
                                apprnc [ "mc" ], apprnc [ "sh" ], apprnc [ "shc" ],
                                apprnc [ "rg" ], apprnc [ "rc" ], apprnc [ "pt" ],
                                apprnc [ "pc" ], apprnc [ "bt" ], apprnc [ "bc" ])

    def  update_crt ( self , uid ):
        одежда  = []
        для  ТМП  в  себе . сервер . Redis . smembers ( f "uid: { uid } : items" ):
            если  сам . сервер . Redis . lindex ( f "uid: { uid } : items: { tmp } " , 0 ) ==  "cls" :
                если  "_"  в  одежде :
                    одежда . append ( tmp . split ( "_" ) [ 0 ])
                еще :
                    одежда . добавить ( тмп )
        внешность  =  я . сервер . get_appearance ( uid )
        если  не  внешний вид :
            вернуть  0
        пол  =  "мальчик",  если  внешность [ "g" ] ==  1  еще  "девочка"
        crt  =  0
        для  ткани  в  одежде :
            для  _категории  в  себе . список одежды [ пол ]:
                за  предмет  в  себе . список одежды [ пол ] [ _category ]:
                    если  предмет  ==  ткань :
                        пункт  =  себя . список одежды [ пол ] [ _категория ] [ ткань ]
                        crt  + =  item [ "rating" ]
                        перерыв
        сам . сервер . Redis . set ( f "uid: { uid } : crt" , crt )
        возврат  элт

    def  get_category ( собственная личность , ткань , пол ):
        если  "_"  в  ткани :
            ткань  =  ткань . split ( "_" ) [ 0 ]
        для  категории  в  себе . список одежды [ пол ]:
            за  предмет  в  себе . список одежды [ пол ] [ категория ]:
                если  предмет  ==  ткань :
                    возвращаемая  категория
        возврат  Нет
 ведение журнала импорта


 Модуль класса ():
    def  __init__ ( self ):
        сам . команды  = {}

    def  on_message ( self , msg , client ):
        команда  =  msg [ 1 ]. сплит ( "." ) [ 1 ]
        если  команда  не  в  себе . команды :
            каротаж . предупреждение ( f "Команда { msg [ 1 ] } не найдена" )
            возвращение
        сам . команды [ команда ] ( MSG , клиент )
из  модулей . base_module  импорта  модуля
импорт  конст

class_name  =  "Billing"


Класс  биллинга ( Модуль ):
    префикс  =  "b"

    def  __init__ ( self , server ):
        сам . сервер  =  сервер
        сам . Команды  = { "chkprchs" : самостоятельно . check_purchase ,
                         "бс" : сам . buy_silver }

    def  check_purchase ( self , msg , client ):
        если  не  конст . FREE_GOLD :
            возвращение
        amount  =  int ( msg [ 2 ] [ "prid" ]. split ( "pack" ) [ 1 ]) * 100
        user_data  =  self . сервер . get_user_data ( client . uid )
        gold  =  user_data [ "gld" ] +  сумма
        сам . сервер . Redis . set ( f "uid: { client . uid } : gld" , gold )
        res  = { "gld" : gold , "slvr" : user_data [ "slvr" ],
               "enrg" : user_data [ "enrg" ], "emd" : user_data [ "emd" ]}
        клиент . отправить ([ "ntf.res" , { "res" : res }])
        клиент . отправить ([ "b.ingld" , { "ingld" : сумма }])

    def  buy_silver ( self , msg , client ):
        user_data  =  self . сервер . get_user_data ( client . uid )
        if  user_data [ "gld" ] <  msg [ 2 ] [ "gld" ]:
            возвращение
        сам . сервер . Redis . set ( f "uid: { client . uid } : gld" ,
                              user_data [ "gld" ] -  msg [ 2 ] [ "gld" ])
        сам . сервер . Redis . set ( f "uid: { client . uid } : slvr" ,
                              user_data [ "slvr" ] +  msg [ 2 ] [ "gld" ] *  100 )
        res  = { "gld" : user_data [ "gld" ] -  msg [ 2 ] [ "gld" ],
               "slvr" : user_data [ "slvr" ] +  msg [ 2 ] [ "gld" ] *  100 ,
               "enrg" : user_data [ "enrg" ], "emd" : user_data [ "emd" ]}
        клиент . отправить ([ "ntf.res" , { "res" : res }])
        клиент . отправить ([ "b.inslv" , { "inslv" : msg [ 2 ] [ "gld" ] *  100 }])
 ведение журнала импорта
импорт  ОС
 дата импорта время

class_name  =  "ClientError"


Класс  ClientError ():
    prefix  =  "клерр"

    def  __init__ ( self , server ):
        сам . сервер  =  сервер

    def  on_message ( self , msg , client ):
        log  =  msg [ 2 ] [ "log" ]
        message  =  msg [ 2 ] [ "message" ]
        если  не  ос . путь . isdir ( f "errors / { client . uid } " ):
            ос . makedirs ( f "errors / { client . uid } " )
        имя файл  =  дата и время . Дата и время . сейчас (). strftime ( "% d.% m.% Y_% H:% M:% S.log" )
        if  len ( os . listdir ( f "errors / { client . uid } " )) > =  100 :
            возвращение
        с  открытым ( f "errors / { client . uid } / { filename } " , "w" ,
                  encoding = 'utf-8' ) как  файл :
            файл . написать ( сообщение + " \ r \ n " )
            файл . написать ( журнал )
        каротаж . ошибка ( f "Client { client . uid } error" )
из  модулей . base_module  импорта  модуля

class_name  =  "Конкурс"


 Конкурс класса ( Модуль ):
    prefix  =  "ctmr"

    def  __init__ ( self , server ):
        сам . сервер  =  сервер
        сам . command  = { "get" : self . get_top }

    def  get_top ( self , msg , client ):
        клиент . отправить ([ "ctmr.get" , { "tu" : { "snowboardRating" : []}}])
 время импорта
из  модулей . base_module  импорта  модуля
импорт  утилит . bot_common

class_name  =  "Компонент"


 Компонент класса ( Модуль ):
    prefix  =  "cp"

    def  __init__ ( self , server ):
        сам . сервер  =  сервер
        сам . command  = { "cht" : self . чат , "м" : сам . модерация ,
                         "мс" : сам . сообщение }
        сам . привилегии  =  себя . сервер . синтаксический анализатор . parse_privileges ()
        сам . mute  = {}

    def  chat ( self , msg , client ):
        подкоманда  =  msg [ 1 ]. сплит ( "." ) [ 2 ]
        if  subcommand  ==  "sm" :   # отправить сообщение
            сообщ . pop ( 0 )
            если  клиент . Uid  в  себе . немой :
                time_left  =  self . немой [ клиент . UID ] - время . время ()
                если  time_left  >  0 :
                    клиент . отправить ([ "cp.ms.rsm" , { "txt" : "У вас мут ещё на"
                                                      f " { int ( time_left ) } "
                                                      "секунд" }])
                    возвращение
                еще :
                    дель  самоуправления . немой [ клиент . UID ]
            if  msg [ 1 ] [ "msg" ] [ "cid" ]:
                для  идентификатора  в  сообщении [ 1 ] [ "msg" ] [ "cid" ]. разделить ( "_" ):
                    для  ТМП  в  себе . сервер . онлайн . copy ():
                        если  тмп . uid  ! =  uid :
                            Продолжать
                        TMP . отправить ( сообщение )
                        перерыв
            еще :
                если  "msg"  в  msg [ 1 ] [ "msg" ] и \
                   msg [ 1 ] [ "msg" ] [ "msg" ]. начинается с ( "!" ):
                    вернуть  себя . system_command ( msg [ 1 ] [ "msg" ] [ "msg" ], клиент )
                для  ТМП  в  себе . сервер . онлайн . copy ():
                    если  тмп . room  ==  msg [ 1 ] [ "rid" ]:
                        TMP . отправить ( сообщение )

     модерация def ( self , msg , client ):
        подкоманда  =  msg [ 1 ]. сплит ( "." ) [ 2 ]
        if  subcommand  ==  "ar" :   # запрос на доступ
            user_data  =  self . сервер . get_user_data ( client . uid )
            if  user_data [ "role" ] > =  self . привилегии [ msg [ 2 ] [ "pvlg" ]]:
                успех  =  правда
            еще :
                успех  =  Ложь
            клиент . отправить ([ "cp.m.ar" , { "pvlg" : msg [ 2 ] [ "pvlg" ],
                                     "sccss" : success }])
        elif  subcommand  ==  "bu" :
            uid  =  msg [ 2 ] [ "uid" ]
            вернуть  себя . ban_user ( uid , клиент )

    def  ban_user ( self , uid , client ):
        user_data  =  self . сервер . get_user_data ( client . uid )
        если  user_data [ "role" ] <  self . привилегии [ "AVATAR_BAN" ]:
            возвращение
        uid_user_data  =  self . сервер . get_user_data ( uid )
        if  uid_user_data [ "role" ] >  2 :
            возвращение
        Redis  =  себя . сервер . Redis
        забанен  =  редис . get ( f "uid { uid } : banned" )
        если  забанен :
            клиент . send ([ "cp.ms.rsm" , { "txt" : f "U UID { uid } уже есть бан"
                                              f "от администратора { banned } " }])
            возвращение
        Redis . set ( f "uid: { uid } : banned" , client . uid )
        ban_time  =  int ( time . time () * 1000 )
        Redis . set ( f "uid: { uid } : ban_time" , ban_time )
        для  ТМП  в  себе . сервер . онлайн . copy ():
            если  тмп . uid  ! =  uid :
                Продолжать
            TMP . отправить ([ 10 , «Пользователь забанен» ,
                      { "duration" : 999999 , "banTime" : ban_time ,
                       "notes" : "" , "reviewerId" : клиент . uid , "reasonId" : 0 ,
                       "unbanType" : "none" , "leftTime" : 0 , "id" : None ,
                       "reviewState" : 1 , "userId" : uid ,
                       "moderatorId" : клиент . uid }], type_ = 2 )
            TMP . подключение . отключение ( 2 )
            перерыв
        клиент . send ([ "cp.ms.rsm" , { "txt" : f "UID { uid } получил бан" }])

    def  unban_user ( self , uid , client ):
        user_data  =  self . сервер . get_user_data ( client . uid )
        если  user_data [ "role" ] <  self . привилегии [ "AVATAR_BAN" ]:
            возвращение
        Redis  =  себя . сервер . Redis
        забанен  =  редис . get ( f "uid: { uid } : banned" )
        если  не  запрещено :
            клиент . send ([ "cp.ms.rsm" , { "txt" : f "U UID { uid } нет бана" }])
            возвращение
        Redis . удалить ( f "uid: { uid } : banned" )
        Redis . удалить ( f "uid: { uid } : ban_time" )
        клиент . send ([ "cp.ms.rsm" , { "txt" : f "Снят бан UID { uid } от"
                                          f "администратора { banned } " }])

     сообщение def ( self , msg , client ):
        подкоманда  =  msg [ 1 ]. сплит ( "." ) [ 2 ]
        если  субкоманда  ==  «SMM» :   # отправить сообщение модератора
            user_data  =  self . сервер . get_user_data ( client . uid )
            если  user_data [ "role" ] <  self . привилегии [ "MESSAGE_TO_USER" ]:
                возвращение
            uid  =  msg [ 2 ] [ "rcpnts" ]
            message  =  msg [ 2 ] [ "txt" ]
            sccss  =  False
            для  ТМП  в  себе . сервер . онлайн . copy ():
                если  тмп . uid  ==  uid :
                    TMP . отправить ([ "cp.ms.rmm" , { "sndr" : client . uid ,
                                            "TXT" : сообщение }])
                    sccss  =  True
                    перерыв
            клиент . отправить ([ "cp.ms.smm" , { "sccss" : sccss }])

    def  system_command ( self , msg , client ):
        команда  =  сообщение [ 1 :]
        если  ""  в  команде :
            команда  =  команда . split ( "" ) [ 0 ]
        если  команда  ==  "ssm" :
            вернуть  себя . send_system_message ( сообщение , клиент )
        elif  command  ==  "mute" :
            вернуть  себя . mute_player ( MSG , клиент )
         команда  elif ==  "бан" :
            UID  =  MSG . split () [ 1 ]
            вернуть  себя . ban_user ( uid , клиент )
         команда  elif ==  "unban" :
            UID  =  MSG . split () [ 1 ]
            вернуть  себя . unban_user ( uid , клиент )
         команда  elif ==  "сброс" :
            UID  =  MSG . split () [ 1 ]
            вернуть  себя . reset_user ( uid , клиент )

    def  send_system_message ( self , msg , client ):
        user_data  =  self . сервер . get_user_data ( client . uid )
        если  user_data [ "role" ] <  self . привилегии [ "SEND_SYSTEM_MESSAGE" ]:
            вернуть  себя . no_permission ( клиент )
        сообщение  =  сообщ . split ( "! ssm" ) [ 1 ]
        для  ТМП  в  себе . сервер . онлайн . copy ():
            TMP . отправить ([ "cp.ms.rsm" , { "txt" : сообщение }])

    def  mute_player ( self , msg , client ):
        user_data  =  self . сервер . get_user_data ( client . uid )
        если  user_data [ "role" ] <  self . привилегии [ "CHAT_BAN" ]:
            вернуть  себя . no_permission ( клиент )
        UID  =  MSG . split () [ 1 ]
        минуты  =  int ( msg . split () [ 2 ])
        apprnc  =  self . сервер . get_appearance ( uid )
        если  не  apprnc :
            клиент . отправить ([ "cp.ms.rsm" , { "txt" : "Игрок не найден" }])
            возвращение
        сам . немой [ uid ] =  время . время () + минуты * 60
        для  ТМП  в  себе . сервер . онлайн . copy ():
            если  тмп . uid  ! =  uid :
                Продолжать
            TMP . отправить ([ "cp.m.bccu" , { "bcu" : { "notes" : "" , "reviewerId" : "0" ,
                                            "mid" : "0" , "id" : нет ,
                                            "reviewState" : 1 , "userId" : uid ,
                                            "mbt" : int ( time . time () * 1000 ),
                                            "mbd" : минуты ,
                                            "categoryId" : 14 }}])
            перерыв
        клиент . send ([ "cp.ms.rsm" , { "txt" : f "Игроку { apprnc [ 'n' ] } выдан мут"
                                          f "на { минут } минут" }])

    def  reset_user ( self , uid , client ):
        user_data  =  self . сервер . get_user_data ( client . uid )
        если  user_data [ "role" ] <  5 :
            вернуть  себя . no_permission ( клиент )
        apprnc  =  self . сервер . get_appearance ( uid )
        если  не  apprnc :
            клиент . отправить ([ "cp.ms.rsm" , { "txt" : "Аккаунт и так сброшен" }])
            возвращение
        для  ТМП  в  себе . сервер . онлайн . copy ():
            если  тмп . uid  ! =  uid :
                Продолжать
            TMP . подключение . отключение ( 2 )
            перерыв
        утилит . bot_common . reset_account ( self . server . redis , uid )
        клиент . отправить ([ "cp.ms.rsm" , { "txt" : f "Аккаунт { uid } был сброшен" }])

    def  no_permission ( self , client ):
        клиент . send ([ "cp.ms.rsm" , { "txt" : "У вас недостаточно прав, чтобы"
                                          "выполнить эту команду" }])
из  модулей . base_module  импорта  модуля

class_name  =  "Craft"


Класс  Craft ( Модуль ):
    prefix  =  "crt"

    def  __init__ ( self , server ):
        сам . сервер  =  сервер
        сам . command  = { "bc" : self . buy_component , "prd" : self . производить }
        сам . craft_items  =  сервер . синтаксический анализатор . parse_craft ()

    def  buy_component ( self , msg , client ):
        buy_for  =  msg [ 2 ] [ "itId" ]
        если  buy_for  не  в  себе . craft_items :
            возвращение
        to_buy  = {}
        золото  =  0
        Redis  =  себя . сервер . Redis
        предметы  =  себя . сервер . game_items [ "loot" ]
        для  элемента  в  сообщении [ 2 ] [ "cmIds" ]:
            цена  =  предметы [ вещь ] [ "золото" ] / 100
            иметь  =  Redis . lindex ( f "uid: { client . uid } : items: { item } " , 1 )
            если  не  иметь :
                иметь  =  0
            еще :
                иметь  =  int ( иметь )
            сумма  =  себя . craft_items [ buy_for ] [ "items" ] [ item ] -  есть
            если  сумма  <=  0 :
                Продолжать
            золото  + =  int ( rd ( цена  *  сумма ))
            to_buy [ item ] =  количество
        user_data  =  self . сервер . get_user_data ( client . uid )
        if  user_data [ "gld" ] <  gold :
            возвращение
        Redis . set ( f "uid: { client . uid } : gld" , user_data [ "gld" ] - gold )
        compIts  = []
        для  элемента  в  сообщении [ 2 ] [ "cmIds" ]:
            сам . сервер . inv [ клиент . UID ]. add_item ( item , "lt" , to_buy [ item ])
            СООТВЕТСТВУЕТ . append ({ "c" : to_buy [ item ], "iid" : "" , "tid" : item })
        user_data  =  self . сервер . get_user_data ( client . uid )
        клиент . отправить ([ "crt.bc" , { "res" : { "gld" : user_data [ "gld" ],
                                        "slvr" : user_data [ "slvr" ],
                                        "enrg" : user_data [ "enrg" ],
                                        "emd" : user_data [ "emd" ]},
                                "itId" : buy_for , "compIts" : compIts }])

    def  производить ( самостоятельно , MSG , клиент ):
        itId  =  msg [ 2 ] [ "itId" ]
        count  =  1
        если  бы  не  в  себе . craft_items :
            возвращение
        Redis  =  себя . сервер . Redis
        за  предмет  в  себе . craft_items [ itId ] [ "items" ]:
            иметь  =  Redis . lindex ( f "uid: { client . uid } : items: { item } " , 1 )
            если  не  иметь :
                возвращение
            иметь  =  int ( иметь )
            если  есть  <  я . craft_items [ itId ] [ "items" ] [ item ]:
                возвращение
        за  предмет  в  себе . craft_items [ itId ] [ "items" ]:
            сумма  =  себя . craft_items [ itId ] [ "items" ] [ item ]
            сам . сервер . inv [ клиент . UID ]. take_item ( позиция , сумма )
        если  "craftedId"  в  себе . craft_items [ itId ]:
            считать  =  себя . craft_items [ itId ] [ "count" ]
            itId  =  self . craft_items [ itId ] [ "craftedId" ]
        если бы  это было  в  себе . сервер . game_items [ "game" ]:
            type_  =  "гм"
        Элиф  itId  в  себе . сервер . game_items [ "loot" ]:
            type_  =  "lt"
        Элиф  itId  в  себе . сервер . модули [ "фрн" ]. frn_list :
            type_  =  "frn"
        сам . сервер . inv [ клиент . UID ]. add_item ( itId , type_ , count )
        inv  =  self . сервер . inv [ клиент . UID ]. получить ()
        клиент . send ([ "crt.prd" , { "inv" : inv , "crIt" : { "c" : count , "lid" : "" ,
                                                      "tid" : itId }}])


def  rd ( x , y = 0 ):
    '' 'Классическое математическое округление Возницей' ''
    m  =  int ( '1' + '0' * y )   # множитель - сколько позиций справа
    q  =  x * m   # сдвиг вправо на множитель
    c  =  int ( q )   # новый номер
    i  =  int (( q - c ) * 10 )   # номер индикатора справа
    если  я  > =  5 :
        с  + =  1
    возврат  с / м
 время импорта
импорт  потоков
из  модулей . base_module  импорта  модуля

class_name  =  "Событие"


Класс  Event ( Модуль ):
    префикс  =  "ev"

    def  __init__ ( self , server ):
        сам . сервер  =  сервер
        сам . command  = { "get" : self . get_events , "gse" : self . get_self_event ,
                         "ЭЛТ" : сам . create_event ,
                         "CSE" : сам . close_self_event ,
                         "Эви" : сам . get_event_info }
        сам . события  = {}
        нить  =  нарезание резьбы . Thread ( цель = сам . _Background )
        нить . daemon  =  True
        нить . начало ()

    def  get_events ( self , msg , client ):
        evts  = []
        для  UID  в  себе . события :
            apprnc  =  self . сервер . get_appearance ( uid )
            если  не  apprnc :
                Продолжать
            evts . добавить ( self . _get_event ( uid ))
        клиент . отправить ([ "ev.get" , { "c" : - 1 , "tg" : "" , "evlst" : evts }])

    def  get_self_event ( self , msg , client ):
        если  клиент . Uid  не  в  себе . события :
            вернуть  клиента . отправить ([ "ev.gse" , {}])
        событие  =  себя . события [ клиент . UID ]
        клиент . send ([ "ev.gse" , { "ev" : self . _get_event ( event , client . uid )}])

    def  create_event ( self , msg , client ):
        если  клиент . Uid  в  себе . события :
            возвращение
        ev  =  msg [ 2 ] [ "ev" ]
        duration  =  int ( msg [ 2 ] [ "evdrid" ]. split ( "eventDuration" ) [ 1 ])
        если  ev [ "r" ] >  13  или  ev [ "c" ] ==  3 :
            user_data  =  self . сервер . get_user_data ( client . uid )
            привилегии  =  себя . сервер . модули [ "cp" ]. привилегии
            если  user_data [ "роль" ] <  привилегии [ "CREATE_MODERATOR_EVENT" ]:
                возвращение
        event  = { "name" : ev [ "tt" ], "description" : ev [ "ds" ],
                 "start" : int ( time . time ()), "uid" : клиент . UID ,
                 "отделка" : int ( время . время () + продолжительность * 60 ),
                 "min_lvl" : ev [ "ml" ], "category" : ev [ "c" ], "active" : ev [ "ac" ],
                 "rating" : ev [ "r" ]}
        если  не  ev [ "l" ]:
            событие [ "location" ] =  "гостиная"
        еще :
            событие [ "location" ] =  ev [ "l" ]
        сам . события [ клиент . uid ] =  событие
        user_data  =  self . сервер . get_user_data ( client . uid )
        клиент . send ([ "ev.crt" , { "ev" : self . _get_event ( client . uid ),
                                "res" : { "gld" : user_data [ "gld" ],
                                        "slvr" : user_data [ "slvr" ],
                                        "enrg" : user_data [ "enrg" ],
                                        "emd" : user_data [ "emd" ]},
                                "evtg" : []}])

    def  close_self_event ( self , msg , client ):
        если  клиент . Uid  не  в  себе . события :
            возвращение
        дель  самоуправления . события [ клиент . UID ]
        клиент . отправить ([ "ev.cse" , {}])

    def  get_event_info ( self , msg , client ):
        id_  =  str ( msg [ 2 ] [ "id" ])
        если  id_  не  в  себе . события :
            возвращение
        событие  =  себя . _get_event ( id_ )
        apprnc  =  self . сервер . get_appearance ( id_ )
        clths  =  self . сервер . get_clothes ( id_ , type_ = 2 )
        клиент . отправить ([ "ev.evi" , { "ev" : событие ,
                                "plr" : { "uid" : id_ , "apprnc" : apprnc ,
                                        "clths" : clths },
                                "id" : int ( id_ )}])

    def  _get_event ( self , uid ):
        событие  =  себя . события [ uid ]
        apprnc  =  self . сервер . get_appearance ( uid )
        type_  =  0,  если  событие [ "location" ] ==  "гостиная"  еще  1
        return { "tt" : событие [ "name" ], "ds" : событие [ "description" ],
                "st" : событие [ "start" ], "ft" : событие [ "finish" ], "uid" : uid ,
                "l" : событие [ "location" ], "id" : int ( uid ), "unm" : apprnc [ "n" ],
                "ac" : событие [ "активный" ], "c" : событие [ "категория" ],
                "ci" : 0 , "fo" : False , "r" : событие [ "rating" ], "lg" : 30 ,
                "tp" : type_ , "ml" : event [ "min_lvl" ]}

    def  _background ( self ):
        пока  верно :
            для  UID  в  себе . события . copy ():
                если  время . время () -  само . events [ uid ] [ "finish" ] >  0 :
                    дель  самоуправления . события [ uid ]
            время . сон ( 60 )
из  модулей . base_module  импорта  модуля
из  модулей . импорт локации  gen_plr 

class_name  =  "Мебель"


 Мебель класса ( Модуль ):
    prefix  =  "frn"

    def  __init__ ( self , server ):
        сам . сервер  =  сервер
        сам . команды  = { "сохранить" : сами . save_layout , "купить" : самостоятельно . купить }
        сам . frn_list  =  сервер . синтаксический анализатор . parse_furniture ()

    def  save_layout ( self , msg , client ):
        комната  =  сообщение [ 0 ]. разделить ( "_" )
        UID  =  клиент . UID
        если  комната [ 1 ] ! =  клиент . UID :
            возвращение
        для  элемента  в  сообщении [ 2 ] [ "f" ]:
            если  элемент [ "t" ] ==  0 :
                сам . type_add ( элемент , комната , UID )
            если  элемент [ "t" ] ==  1 :
                сам . type_update ( item , room , uid )
            elif  item [ "t" ] ==  2 :
                сам . type_remove ( элемент , комната , UID )
            elif  item [ "t" ] ==  3 :
                сам . type_replace_door ( item , room , uid )
        inv  =  self . сервер . inv [ uid ]. получить ()
        room_inf  =  self . сервер . Redis . lrange ( f "rooms: { uid } : { room [ 2 ] } " , 0 , - 1 )
        room_items  =  self . сервер . get_room_items ( uid , комната [ 2 ])
        сам . update_hrt ( UID )
        CI  =  gen_plr ( клиент , самостоятельно . Сервер ) [ "ДИ" ]
        клиент . отправить ([ "frn.save" , { "inv" : inv , "ci" : ci ,
                                  "hs" : { "f" : room_items , "w" : 13 ,
                                         "id" : комната [ 2 ], "l" : 13 ,
                                         "lev" : int ( room_inf [ 1 ]),
                                         "nm" : room [ 0 ]}}])

    def  type_add ( self , item , room , uid ):
        предметы  =  себя . сервер . Redis . smembers ( f "rooms: { uid } : { room [ 2 ] } : items" )
        если  не  я . сервер . inv [ uid ]. take_item ( item [ "tpid" ]):
            возвращение
        если  есть ( ext  в  item [ "tpid" ]. lower () для  ext  в [ "wll" , "wall" ]):
            стены  = []
            для  стены  в [ "wall" , "wll" ]:
                для  room_item  в  элементах :
                    если  стена  в  комнате . ниже ():
                        сам . del_item ( room_item , room [ 2 ], uid )
                        tmp  =  room_item . split ( "_" ) [ 0 ]
                        если  тмп  не  в  стенах :
                            стены . добавить ( тмп )
                            сам . сервер . inv [ uid ]. add_item ( tmp , "frn" )
            item [ "x" ] =  0.0
            item [ "y" ] =  0.0
            item [ "z" ] =  0.0
            item [ "d" ] =  3
            сам . add_item ( элемент , комната [ 2 ], uid )
            item [ "x" ] =  13,0
            item [ "d" ] =  5
            item [ "oid" ] + =  1
            сам . add_item ( элемент , комната [ 2 ], uid )
        elif  any ( ext  в  item [ "tpid" ]. lower () для  ext  в [ "flr" , "floor" ]):
            для  пола  в [ "flr" , "floor" ]:
                для  room_item  в  элементах :
                    если  пол  в  room_item . ниже ():
                        сам . del_item ( room_item , room [ 2 ], uid )
                        tmp  =  room_item . split ( "_" ) [ 0 ]
                        сам . сервер . inv [ uid ]. add_item ( tmp , "frn" )
            item [ "x" ] =  0.0
            item [ "y" ] =  0.0
            item [ "z" ] =  0.0
            item [ "d" ] =  5
            сам . add_item ( элемент , комната [ 2 ], uid )

    def  type_update ( self , item , room , uid ):
        Redis  =  себя . сервер . Redis
        items  =  redis . smembers ( f "rooms: { uid } : { room [ 2 ] } : items" )
        name  =  f " { item [ 'tpid' ] } _ { item [ 'oid' ] } "
        если  имя  в  пунктах :
            избавиться  =  Redis . lindex ( f "rooms: { uid } : { room [ 2 ] } : items: { name } " , 4 )
            если  избавиться :
                item [ "rid" ] =  избавиться
            сам . del_item ( имя , комната [ 2 ], uid )
            сам . add_item ( элемент , комната [ 2 ], uid )
        еще :
            если  не  я . сервер . inv [ uid ]. take_item ( item [ "tpid" ]):
                возвращение
            сам . add_item ( элемент , комната [ 2 ], uid )

    def  type_remove ( self , item , room , uid ):
        предметы  =  себя . сервер . Redis . smembers ( f "rooms: { uid } : { room [ 2 ] } : items" )
        name  =  f " { item [ 'tpid' ] } _ { item [ 'oid' ] } "
        если  имя  не  в  пунктах :
            возвращение
        сам . del_item ( имя , комната [ 2 ], uid )
        сам . сервер . inv [ uid ]. add_item ( item [ "tpid" ], "frn" )

    def  type_replace_door ( self , item , room , uid ):
        предметы  =  себя . сервер . Redis . smembers ( f "rooms: { uid } : { room [ 2 ] } : items" )
        найдено  =  нет
        для  ТМП  в  пунктах :
            oid  =  int ( tmp . split ( "_" ) [ 1 ])
            if  oid  ==  item [ "oid" ]:
                найдено  =  тмп
                перерыв
        если  не  найден :
            возвращение
        если  не  я . сервер . inv [ uid ]. take_item ( item [ "tpid" ]):
            возвращение
        данные  =  себя . сервер . Redis . lrange ( f "rooms: { uid } : { room [ 2 ] } : items: { found } " ,
                                        0 , - 1 )
        если  длина ( данные ) <  5 :
            рид  =  нет
        еще :
            рид  =  данные [ 4 ]
        сам . del_item ( найдено , комната [ 2 ], uid )
        сам . сервер . inv [ uid ]. add_item ( найдено . split ( "_" ) [ 0 ], "frn" )
        предмет . update ({ "x" : float ( data [ 0 ]), "y" : float ( data [ 1 ]),
                     "z" : float ( данные [ 2 ]), "d" : int ( данные [ 3 ]),
                     "рид" : рид })
        сам . add_item ( элемент , комната [ 2 ], uid )

    def  buy ( self , msg , client ):
        item  =  msg [ 2 ] [ "tpid" ]
        количество  =  msg [ 2 ] [ "cnt" ]
        UID  =  клиент . UID
        если  предмет  не  в  себе . frn_list :
            возвращение
        user_data  =  self . сервер . get_user_data ( uid )
        золото  =  сам . frn_list [ item ] [ "gold" ] * количество
        серебро  =  сам . frn_list [ item ] [ "silver" ] * количество
        if  user_data [ "gld" ] <  gold  или  user_data [ "slvr" ] <  silver :
            возвращение
        сам . сервер . Redis . set ( f "uid: { uid } : gld" , user_data [ "gld" ] -  gold )
        сам . сервер . Redis . set ( f "uid: { uid } : slvr" , user_data [ "slvr" ] -  серебро )
        сам . сервер . inv [ uid ]. add_item ( элемент , "франк" , сумма )
        amount  =  int ( self . server . redis . lindex ( f "uid: { uid } : items: { item } " , 1 ))
        клиент . send ([ "ntf.inv" , { "it" : { "c" : amount , "iid" : "" , "tid" : item }}])
        user_data  =  self . сервер . get_user_data ( uid )
        клиент . отправить ([ "ntf.res" , { "res" : { "gld" : user_data [ "gld" ],
                                         "slvr" : user_data [ "slvr" ],
                                         "enrg" : user_data [ "enrg" ],
                                         "emd" : user_data [ "emd" ]}}])

    def  add_item ( self , item , room , uid ):
        сам . сервер . Redis . sadd ( f "rooms: { uid } : { room } : items" ,
                               f " { item [ 'tpid' ] } _ { item [ 'oid' ] } " )
        если  "избавиться"  в  пункте :
            сам . сервер . Redis . rpush ( f "комнаты: { uid } : { комната } : предметы:"
                                    f " { item [ 'tpid' ] } _ { item [ 'oid' ] } " , item [ "x" ],
                                    item [ "y" ], item [ "z" ], item [ "d" ],
                                    пункт [ "избавить" ])
        еще :
            сам . сервер . Redis . rpush ( f "комнаты: { uid } : { комната } : предметы:"
                                    f " { item [ 'tpid' ] } _ { item [ 'oid' ] } " , item [ "x" ],
                                    item [ "y" ], item [ "z" ], item [ "d" ])

    def  del_item ( self , item , room , uid ):
        предметы  =  себя . сервер . Redis . smembers ( f "rooms: { uid } : { room } : items" )
        если  товар  не  в  товарах :
            возвращение
        сам . сервер . Redis . srem ( f "rooms: { uid } : { room } : items" , item )
        сам . сервер . Redis . удалить ( f "rooms: { uid } : { room } : items: { item } " )

    def  update_hrt ( self , uid ):
        Redis  =  себя . сервер . Redis
        hrt  =  0
        для  комнаты  в  Redis . smembers ( f "rooms: { uid } " ):
            для  элемента  в  Redis . smembers ( f "rooms: { uid } : { room } : items" ):
                элемент  =  элемент . split ( "_" ) [ 0 ]
                если  предмет  не  в  себе . frn_list :
                    Продолжать
                хрю  + =  самостоятельно . frn_list [ item ] [ "rating" ]
        Redis . set ( f "uid: { uid } : hrt" , hrt )
        возврат  HRT
из  модулей . Расположение  импорта  Расположение , gen_plr
импорт  общего
импорт  конст

class_name  =  "Дом"


Класс  Дом ( Расположение ):
    префикс  =  "ч"

    def  __init__ ( self , server ):
        супер (). __init__ ( сервер )
        сам . команды . update ({ "minfo" : self . get_my_info , "gr" : self . get_room ,
                              "oinfo" : self . владелец_инфо ,
                              "ioinfo" : сам . init_owner_info })

    def  get_my_info ( self , msg , client ):
        apprnc  =  self . сервер . get_appearance ( client . uid )
        если  не  apprnc :
            клиент . отправить ([ "h.minfo" , { "has.avtr" : False }])
            возвращение
        user_data  =  self . сервер . get_user_data ( client . uid )
        inv  =  self . сервер . inv [ клиент . UID ]. получить ()
        CS  =  Self . сервер . get_clothes ( client . uid , type_ = 1 )
        комнаты  = []
        за  предмет  в  себе . сервер . Redis . smembers ( f "rooms: { client . uid } " ):
            номер  =  самостоятельно . сервер . Redis . lrange ( f "rooms: { client . uid } : { item } " ,
                                            0 , - 1 )
            комнаты . append ({ "f" : self . server . get_room_items ( client . uid , item ),
                          "w" : 13 , "id" : item , "lev" : int ( room [ 1 ]), "l" : 13 ,
                          "nm" : комната [ 0 ]})
        ac  = {}
        за  предмет  в  себе . сервер . достижения :
            ac [ item ] = { "p" : 0 , "nWct" : 0 , "l" : 3 , "aId" : item }
        tr  = {}
        за  предмет  в  себе . сервер . трофеи :
            tr [ item ] = { "trrt" : 0 , "trcd" : 0 , "trid" : item }
        ГНР  =  gen_plr ( клиент , самостоятельно . сервер )
        плр . update ({ "cs" : cs , "hs" : { "r" : комнаты , "lt" : 0 }, "inv" : inv ,
                    "onl" : True , "achc" : { "ac" : ac , "tr" : tr }})
        plr [ "res" ] = { "slvr" : user_data [ "slvr" ], "enrg" : user_data [ "enrg" ],
                      "emd" : user_data [ "emd" ], "gld" : user_data [ "gld" ]}
        клиент . отправить ([ "h.minfo" , { "plr" : plr , "tm" : 1 }])
        сам . _perform_login ( клиент )

    def  owner_info ( self , msg , client ):
        если  не  msg [ 2 ] [ "uid" ]:
            возвращение
        plr  =  gen_plr ( msg [ 2 ] [ "uid" ], self . server )
        комнаты  = []
        tmp  =  self . сервер . Redis . smembers ( f "rooms: { msg [ 2 ] [ 'uid' ] } " )
        для  элемента  в  TMP :
            номер  =  самостоятельно . сервер . Redis . lrange ( f "rooms: { msg [ 2 ] [ 'uid' ] } : { item } " ,
                                            0 , - 1 )
            room_items  =  self . сервер . get_room_items ( msg [ 2 ] [ "uid" ], элемент )
            комнаты . append ({ "f" : room_items , "w" : 13 , "l" : 13 , "id" : item ,
                          "lev" : int ( room [ 1 ]), "nm" : room [ 0 ]})
        клиент . send ([ "h.oinfo" , { "ath" : False , "plr" : plr ,
                                 "hs" : { "r" : комнаты , "lt" : 0 }}])

    def  init_owner_info ( self , msg , client ):
        если  не  msg [ 2 ] [ "uid" ]:
            возвращение
        plr  =  gen_plr ( msg [ 2 ] [ "uid" ], self . server )
        клиент . send ( "h.ioinfo" , { "tids" : [], "ath" : False , "plr" : plr })

    def  get_room ( self , msg , client ):
        room  =  f " { msg [ 2 ] [ 'lid' ] } _ { msg [ 2 ] [ 'gid' ] } _ { msg [ 2 ] [ 'rid' ] } "
        если  клиент . комната :
            префикс  =  общий . get_prefix ( клиент . комната )
            для  ТМП  в  себе . сервер . онлайн . copy ():
                если  тмп . комната  ! =  клиент . комната  или  тмп . UID  ==  клиент . UID :
                    Продолжать
                TMP . send ([ f " { prefix } .r.lv" , { "uid" : client . uid }])
                TMP . отправить ([ клиент . комната , клиент . UID ], тип_ = 17 )
        клиент . комната  =  комната
        клиент . позиция  = ( - 1,0 , - 1,0 )
        клиент . action_tag  =  ""
        клиент . состояние  =  0
        клиент . размерность  =  4
        ГНР  =  gen_plr ( клиент , самостоятельно . сервер )
        для  ТМП  в  себе . сервер . онлайн . copy ():
            если  тмп . комната  ! =  клиент . комната :
                Продолжать
            префикс  =  общий . get_prefix ( клиент . комната )
            TMP . отправить ([ f " { префикс } .r.jn" , { "plr" : plr }])
            TMP . отправить ([ клиент . комната , клиент . UID ], тип_ = 16 )
        клиент . отправить ([ "h.gr" , { "rid" : клиент . комната }])

    def  room ( self , msg , client ):
        подкоманда  =  msg [ 1 ]. сплит ( "." ) [ 2 ]
        if  subcommand  ==  "info" :
            rmmb  = []
            комната  =  сообщение [ 0 ]
            для  ТМП  в  себе . сервер . онлайн . copy ():
                если  тмп . комната  ! =  комната :
                    Продолжать
                rmmb . append ( gen_plr ( tmp , self . server ))
            room_addr  =  f "rooms: { msg [ 2 ] [ 'uid' ] } : { msg [ 2 ] [ 'rid' ] } "
            tmp  =  self . сервер . Redis . Lrange ( room_addr , 0 , - 1 )
            room_items  =  self . сервер . get_room_items ( msg [ 2 ] [ "uid" ],
                                                    msg [ 2 ] [ "rid" ])
            room  = { "f" : room_items , "w" : 13 , "id" : msg [ 2 ] [ "rid" ],
                    "l" : 13 , "lev" : int ( tmp [ 1 ]), "nm" : tmp [ 0 ]}
            клиент . отправить ([ "hrinfo" , { "rmmb" : rmmb , "rm" : комната ,
                                      "evn" : нет }])
         подкоманда  elif ==  "rfr" :
            комната  =  сообщение [ 0 ]. разделить ( "_" ) [ - 1 ]
            room_data  =  self . сервер . Redis . lrange ( f "rooms: { client . uid } : { room } " ,
                                                 0 , - 1 )
            room_items  =  self . сервер . get_room_items ( client . uid , room )
            для  ТМП  в  себе . сервер . онлайн . copy ():
                если  тмп . комната  ! =  сообщение [ 0 ]:
                    Продолжать
                TMP . send ([ "hrrfr" , { "rm" : { "f" : room_items , "w" : 13 , "l" : 13 ,
                                             "lev" : int ( room_data [ 1 ]),
                                             "nm" : room_data [ 0 ]}}])
        еще :
            супер (). комната ( сообщение , клиент )

    def  _perform_login ( self , client ):
        клиент . отправить ([ "cm.new" , { "кампании" : сопзЬ . кампании }])
        клиент . отправить ([ "cp.cht.gbl" , { "blcklst" : { "uids" : []}}])
        клиент . send ([ "nws.hasnews" , { "gnexst" : True , "gnunr" : True }]
 ведение журнала импорта
из  модулей . base_module  импорта  модуля
из  клиента  импорт  клиента
импорт  общего


 Расположение класса ( Модуль ):
    def  __init__ ( self , server ):
        сам . сервер  =  сервер
        сам . command  = { "r" : self . комната }
        сам . actions  = { "ks" : "kiss" , "hg" : "hug" , "gf" : "giveFive" ,
                        "k" : "kickAss" , "sl" : "slap" , "lks" : "longKiss" ,
                        "hs" : "handShake" }

    def  room ( self , msg , client ):
        подкоманда  =  msg [ 1 ]. сплит ( "." ) [ 2 ]
         подкоманда  if в [ "u" , "m" , "k" , "sa" , "sl" , "bd" , "lks" , "hs" ,
                          "ks" , "hg" , "gf" ]:
            сообщ . pop ( 0 )
            if  msg [ 1 ] [ "uid" ] ! =  клиент . UID :
                возвращение
            если  подкоманда  ==  "u" :
                клиент . position  = ( msg [ 1 ] [ "x" ], msg [ 1 ] [ "y" ])
                клиент . direction  =  msg [ 1 ] [ "d" ]
                если  "в"  в  сообщении [ 1 ]:
                    клиент . action_tag  =  msg [ 1 ] [ "at" ]
                еще :
                    клиент . action_tag  =  ""
                клиент . state  =  msg [ 1 ] [ "st" ]
            elif  subcommand  ==  "sa" :
                если  "pntHlRd"  в  msg [ 1 ] [ "at" ]:
                    возвращение
             подкоманда  elif в  себе . действия :
                uid  =  msg [ 1 ] [ "tmid" ]
                гх  =  самостоятельно . сервер . модули [ "рл" ]
                ссылка  =  рл . get_link ( client . uid , uid )
                если  ссылка :
                    р . add_progress ( self . actions [ подкоманда ], ссылка )
            для  ТМП  в  себе . сервер . онлайн :
                если  тмп . UID  ==  клиент . UID  или  TMP . комната  ! =  клиент . комната :
                    Продолжать
                TMP . отправить ( сообщение )
        elif  subcommand  ==  "ra" :
            refresh_avatar ( клиент , самостоятельно . сервер )
        еще :
            каротаж . предупреждение ( f "Команда { msg [ 1 ] } не найдена" )


def  gen_plr ( клиент , сервер ):
    если  isinstance ( клиент , клиент ):
        UID  =  клиент . UID
    еще :
        UID  =  клиент
    apprnc  =  сервер . get_appearance ( uid )
    если  не  apprnc :
        возврат  Нет
    user_data  =  сервер . get_user_data ( uid )
    clths  =  сервер . get_clothes ( uid , type_ = 2 )
    plr  = { "uid" : uid , "apprnc" : apprnc , "clths" : clths ,
           "mbm" : { "ac" : нет , "sk" : "blackMobileSkin" },
           "usrinf" : { "rl" : user_data [ "role" ]}}
    если  isinstance ( клиент , клиент ):
        plr [ "locinfo" ] = { "st" : клиент . состояние , "s" : "127.0.0.1" ,
                          "at" : клиент . action_tag , "d" : клиент . размерность ,
                          «х» : клиент . position [ 0 ], "y" : клиент . положение [ 1 ],
                          "shlc" : True , "pl" : "" , "l" : client . комната }
    plr [ "ci" ] = { "exp" : user_data [ "exp" ], "crt" : user_data [ "crt" ],
                 "hrt" : user_data [ "hrt" ], "fexp" : 0 , "gdc" : 0 , "lgt" : 0 ,
                 "vip" : True , "vexp" : 1965298000 , "vsexp" : 1965298000 ,
                 "vsact" : True , "vret" : 0 , "vfgc" : 0 , "ceid" : 0 , "cmid" : 0 ,
                 "dr" : True , "spp" : 0 , "tts" : нет , "eml" : нет , "ys" : 0 ,
                 "ysct" : 0 , "fak" : нет , "shcr" : True , "gtrfrd" : 0 ,
                 "strfrd" : 0 , "rtrtm" : 0 , "kyktid" : нет , "actrt" : 0 ,
                 "compid" : 0 , "actrp" : 0 , "actrd" : 0 , "shousd" : False ,
                 "rpt" : 0 , "as" : нет , "lvt" : user_data [ "lvt" ],
                 "lrnt" : 0 , "lwts" : 0 , "skid" : нет , "skrt" : 0 , "bcld" : 0 ,
                 "trid" : user_data [ "trid" ], "trcd" : 0 , "sbid" : нет ,
                 "sbrt" : 0 , "plcmt" : {}, "pamns" : { "amn" : []}, "crst" : 0 }
    plr [ "pf" ] = { "pf" : { "jntr" : { "tp" : "jntr" , "l" : 20 , "pgs" : 0 },
                        "phtghr" : { "tp" : "phtghr" , "l" : 20 , "pgs" : 0 },
                        "grdnr" : { "tp" : "grdnr" , "l" : 20 , "pgs" : 0 },
                        "vsgst" : { "tp" : "vsgst" , "l" : 20 , "pgs" : 0 }}}
    возврат  плр


def  refresh_avatar ( клиент , сервер ):
    plr  =  gen_plr ( клиент , сервер )
    префикс  =  общий . get_prefix ( клиент . комната )
    для  тмп  в  сервер . онлайн . copy ():
        если  тмп . комната  ! =  клиент . комната :
            Продолжать
        TMP . отправить ([ f " { префикс } .r.ra" , { "plr" : plr }])
из  модулей . base_module  импорта  модуля
из  модулей . импорт локации  gen_plr 

class_name  =  "Инвентарь"


 Инвентарь класса ( Модуль ):
    префикс  =  "tr"

    def  __init__ ( self , server ):
        сам . сервер  =  сервер
        сам . command  = { "sale" : self . sale_item }

    def  sale_item ( self , msg , client ):
        предметы  =  себя . сервер . game_items [ "game" ]
        tpid  =  msg [ 2 ] [ "tpid" ]
        cnt  =  msg [ 2 ] [ "cnt" ]
        если  tpid  отсутствует  в  элементах  или  "saleSilver"  отсутствует  в  элементах [ tpid ]:
            возвращение
        если  не  я . сервер . inv [ клиент . UID ]. take_item ( tpid , cnt ):
            возвращение
        цена  =  предметы [ tpid ] [ "saleSilver" ]
        user_data  =  self . сервер . get_user_data ( client . uid )
        Redis  =  себя . сервер . Redis
        Redis . set ( f "uid: { client . uid } : slvr" , user_data [ "slvr" ] + цена * cnt )
        ci  =  gen_plr ( client . uid , self . server ) [ "ci" ]
        клиент . отправить ([ "ntf.ci" , { "ci" : ci }])
        inv  =  self . сервер . inv [ клиент . UID ]. получить ()
        клиент . отправить ([ "ntf.inv" , { "inv" : inv }])
        user_data  =  self . сервер . get_user_data ( client . uid )
        клиент . отправить ([ "ntf.res" , { "res" : { "gld" : user_data [ "gld" ],
                                         "slvr" : user_data [ "slvr" ],
                                         "enrg" : user_data [ "enrg" ],
                                         "emd" : user_data [ "emd" ]}}])
импорт  бинасксии
 дата импорта время
от  битовая  импорта  BitArray


def  zero_fill_right_shift ( val , n ):
    return ( val  >>  n ), если  val  > =  0  else (( val  +  0x100000000 ) >>  n )


def  processFrame ( data , client = False ):
    маска  =  данные . читать ( 8 ). UINT
    checkummed_mask  =  1  <<  3
    если  0  ! = ( mask  &  checkummed_mask ):
        контрольная сумма  =  True
    еще :
        контрольная сумма  =  ложь
    если  контрольная сумма :
        контрольная сумма  =  данные . читать ( 32 ). UINT
        old_pos  =  data . позиция
        сообщение  =  данные . читать ( len ( data ) - data . pos )
        данные . pos  =  old_pos
        real_checksum  =  binascii . crc32 ( сообщение . байты ) % ( 1  <<  32 )
        если  контрольная сумма  ! =  real_checksum :
            возврат  Нет
    если  клиент :
        данные . pos  + =  32   # номер сообщения
    тип_  =  данные . читать ( 8 ). ИНТ
    return { "type" : type_ , "msg" : decodeArray ( data )}


def  encodeArray ( data ):
    final_data  =  BitArray ()
    final_data . append ( f "int: 32 = { len ( data ) } " )
    для  элемента  в  данных :
        final_data . append ( encodeValue ( item ))
    вернуть  final_data


def  encodeValue ( data , forDict = False ):
    final_data  =  BitArray ()
    если  данные  не  None :
        final_data . append ( "int: 8 = 0" )
    Элиф  isinstance ( данные , BOOL ):
        final_data . append ( "int: 8 = 1" )
        final_data . append ( f "int: 8 = { int ( data ) } " )
    elif  isinstance ( data , int ):
        если  данные  >  2147483647 :
            final_data . append ( "int: 8 = 3" )
            final_data . append ( f "int: 64 = { data } " )
        еще :
            final_data . append ( "int: 8 = 2" )
            final_data . append ( f "int: 32 = { data } " )
    elif  isinstance ( data , float ):
        final_data . append ( "int: 8 = 4" )
        final_data . append ( f "float: 64 = { data } " )
    Элиф  isinstance ( данные , ул ):
        если  не  forDict :
            final_data . append ( "int: 8 = 5" )
        если  не  все ( ord ( c ) <  128  для  c  в  данных ):   # проверка не ASCII-символов
            длина  =  длина ( data . encode (). hex ()) // 2
        еще :
            длина  =  длина ( данные )
        while ( length  &  4294967168 ) ! =  0 :
            final_data . append ( f "uint: 8 = { length  &  127  |  128 } " )
            length  =  zero_fill_right_shift ( length , 7 )
        final_data . append ( f "uint: 8 = { length  &  127 } " )
        final_data . добавить ( data . encode ())
    elif  isinstance ( данные , дикт ):
        final_data . append ( "int: 8 = 6" )
        final_data . append ( encodeDictionary ( data ))
    elif  isinstance ( данные , список ):
        final_data . append ( "int: 8 = 7" )
        final_data . append ( encodeArray ( data ))
    Элиф  isinstance ( данные , дата и время . Дата и время ):
        final_data . append ( "int: 8 = 8" )
        final_data . append ( f "int: 64 = { int ( data . timestamp () * 1000 ) } " )
    еще :
        поднять  ValueError ( «Не могу закодировать» + str ( тип ( данные )))
    вернуть  final_data


def  encodeDictionary ( data ):
    final_data  =  BitArray ()
    final_data . append ( f "int: 32 = { len ( data ) } " )
    для  элемента  в  данных . ключи ():
        final_data . append ( encodeValue ( item , forDict = True ))
        final_data . append ( encodeValue ( data [ item ]))
    вернуть  final_data


def  decodeArray ( data ):
    результат  = []
    длина  =  данные . читать ( 32 ). ИНТ
    я  =  0
    пока  я  <  длина :
        результат . append ( decodeValue ( data ))
        я  + =  1
    вернуть  результат


def  decodeDictionary ( data ):
    поля  =  данные . читать ( 32 ). ИНТ
    obj  = {}
    я  =  0
    пока  я  <  поля :
        ключ  =  decodeString ( данные )
        obj [ ключ ] =  decodeValue ( данные )
        я  + =  1
    вернуть  объект


def  decodeValue ( данные ):
    dataType  =  data . читать ( 8 ). ИНТ
    if  dataType  ==  0 :   # null
        возврат  Нет
    elif  dataType  ==  1 :   # bool
        если  данные . читать ( 8 ). int :
            верните  True
        еще :
            вернуть  Ложь
    elif  dataType  ==  2 :   # int
        вернуть  данные . читать ( 32 ). ИНТ
    elif  dataType  ==  3 :   # long
        вернуть  данные . читать ( 64 ). ИНТ
    elif  dataType  ==  4 :   # double
        вернуть  данные . читать ( 64 ). поплавок
    elif  dataType  ==  5 :   # строка
        вернуть  decodeString ( данные )
    elif  dataType  ==  6 :   # словарь
        вернуть  decodeDictionary ( data )
    elif  dataType  ==  7 :   # массив
        вернуть  decodeArray ( данные )
    elif  dataType  ==  8 :   # дата
        вернуться  даты и времени . Дата и время . метка времени ( data . read ( 64 ). int / 1000 )
    еще :
        поднять  ValueError ( f "Неверный тип данных: { dataType } " )


def  decodeString ( data ):
    я  =  0
    б  =  данные . читать ( 8 ). UINT
    значение  =  0
    пока  b  &  128  ! =  0 :
        значение  + = ( b  &  127 ) <<  i
        я  + =  7
        если  я  >  35 :
            повысить  исключение ( «Количество переменной длины слишком велико» )
        б  =  данные . читать ( 8 ). UINT
    длина  =  значение  |  б  <<  я
    вернуть  данные . читать ( длина * 8 ). байт . декодировать ()
зафиксироваться
# игровой сервер
битовая
Redis
LXML
# веб
aiohttp
aiohttp_session
aiohttp_jinja2
криптография
# боты
aiogram
импорт  importlib
импорт  потоков
 подпроцесс импорта
 гнездо для импорта
 ведение журнала импорта
 время импорта
импорт  редис
импортировать  исключения
из  клиента  импорт  клиента
из  инвентаря  импорт  инвентарь
от  синтаксического анализатора  импорта  Parser

modules  = [ "client_error" , "house" , "outside" , "user_rating" , "mail" , "avatar" ,
           "location_game" , "отношения" , "social_request" , "user_rating" ,
           «конкуренция» , «мебель» , «биллинг» , «компонент» , «поддержка» ,
           «паспорт» , «игрок» , «статистика» , «магазин» , «мобильный» , «подтвердить» ,
           "ремесло" , "профессия" , "инвентарь" , "событие" ]


def  get_git_revision_short_hash ():
    попробуй :
        вернуться  подпроцесс . check_output ([ "git" , "rev-parse" ,
                                        "--short" , "HEAD" ]). полоса (). декодировать ()
    кроме ( FileNotFoundError , подпроцесс . CalledProcessError ):
        возврат  "Неизвестно"


Класс  Server ():
    def  __init__ ( self , host = "0.0.0.0" , port = 8123 ):
        сам . онлайн  = []
        сам . inv  = {}
        сам . redis  =  redis . Redis ( decode_responses = True )
        сам . parser  =  Parser ()
        сам . конфликты  =  я . синтаксический анализатор . parse_conflicts ()
        сам . достижения  =  себя . синтаксический анализатор . parse_achievements ()
        сам . Трофеи  =  самостоятельно . синтаксический анализатор . parse_trophies ()
        сам . game_items  =  self . синтаксический анализатор . parse_game_items ()
        сам . внешность  =  я . синтаксический анализатор . parse_appearance ()
        сам . modules  = {}
        для  элемента  в  модулях :
            модуль  =  importlib . import_module ( f "modules. { item } " )
            class_  =  getattr ( модуль , модуль . имя_класса )
            сам . модули [ класс_ . префикс ] =  класс_ ( сам )
        сам . revision  =  get_git_revision_short_hash ()
        сам . носок  =  сокет . сокет ( сокет . AF_INET , сокет . SOCK_STREAM )
        сам . носок . setsockopt ( сокет . SOL_SOCKET , сокет . SO_REUSEADDR , 1 )
        сам . носок . связать (( хост , порт ))

    def  listen ( self ):
        сам . носок . слушать ( 5 )
        каротаж . info ( «Сервер готов к приему соединений» )
        нить  =  нарезание резьбы . Thread ( цель = сам . _Background )
        нить . daemon  =  True
        нить . начало ()
        пока  верно :
            клиент , адрес  =  сам . носок . принять ()
            нить  =  нарезание резьбы . Поток ( цель = Клиент ( сам ). Дескриптор ,
                                      args = ( клиент , адрес ))
            нить . daemon  =  True
            нить . начало ()

    def  process_data ( self , data , client ):
        если  не  клиент . UID :
            if  data [ "type" ] ! =  1 :
                клиент . подключение . отключение ( 2 )
            сам . auth ( data [ "msg" ], клиент )
            возвращение
        если  данные [ "тип" ] ==  2 :
            клиент . подключение . отключение ( 2 )
            возвращение
        elif  data [ "type" ] ==  34 :
            префикс  =  data [ "msg" ] [ 1 ]. split ( "." ) [ 0 ]
            если  префикс  не  в  себе . модули :
                каротаж . предупреждение ( f «Команда { данные [ 'msg' ] [ 1 ] } не найдена" )
                возвращение
            сам . модули [ префикс ]. on_message ( data [ "msg" ], клиент )

    def  auth ( self , msg , client ):
        UID  =  себя . Redis . get ( f "auth: { msg [ 2 ] } " )
        если  не  UID :
            клиент . подключение . отключение ( 2 )
            возвращение
        забанен  =  сам . Redis . get ( f "uid: { uid } : banned" )
        если  забанен :
            ban_time  =  int ( self . redis . get ( f "uid: { uid } : ban_time" ))
            клиент . отправить ([ 10 , «Пользователь забанен» ,
                         { "duration" : 999999 , "banTime" : ban_time ,
                          "notes" : "Опа бан" , "reviewerId" : запрещен ,
                          "reasonId" : 0 , "unbanType" : "none" , "leftTime" : 0 ,
                          "id" : нет , "reviewState" : 1 , "userId" : uid ,
                          "moderatorId" : banned }], type_ = 2 )
            клиент . подключение . отключение ( 2 )
            возвращение
        для  ТМП  в  себе . онлайн :
            если  тмп . uid  ==  uid :
                попробуй :
                    TMP . подключение . отключение ( 2 )
                кроме  OSError :
                    проходят
                перерыв
        клиент . UID  =  UID
        сам . онлайн . приложение ( клиент )
        сам . Redis . set ( f "uid: { uid } : lvt" , int ( time . time ()))
        если  UID  не  в  себе . inv :
            сам . inv [ uid ] =  Инвентарь ( self , uid )
        еще :
            сам . inv [ uid ]. истекает  =  нет
        клиент . отправить ([ client . uid , True , False , False ], type_ = 1 )
        клиент . контрольная сумма  =  True

    def  get_user_data ( self , uid ):
        труба  =  себя . Redis . трубопровод ()
        труба . get ( f "uid: { uid } : slvr" ). get ( f "uid: { uid } : enrg" )
        труба . get ( f "uid: { uid } : gld" ). get ( f "uid: { uid } : exp" ). get ( f "uid: { uid } : emd" )
        труба . get ( f "uid: { uid } : lvt" ). get ( f "uid: { uid } : trid" ). get ( f "uid: { uid } : crt" )
        труба . get ( f "uid: { uid } : hrt" ). get ( f "uid: { uid } : role" )
        результат  =  труба . выполнить ()
        если  не  результат [ 0 ]:
            возврат  Нет
        если  результат [ 7 ]:
            crt  =  int ( результат [ 7 ])
        еще :
            КРТ  =  себя . модули [ "а" ]. update_crt ( uid )
        если  результат [ 8 ]:
            hrt  =  int ( результат [ 8 ])
        еще :
            HRT  =  себя . модули [ "фрн" ]. update_hrt ( UID )
        если  результат [ 9 ]:
            роль  =  int ( результат [ 9 ])
        еще :
            роль  =  0
        return { "uid" : uid , "slvr" : int ( result [ 0 ]), "enrg" : int ( result [ 1 ]),
                "gld" : int ( результат [ 2 ]), "exp" : int ( результат [ 3 ]),
                "emd" : int ( результат [ 4 ]), "lvt" : int ( результат [ 5 ]), "crt" : crt ,
                "hrt" : hrt , "trid" : result [ 6 ], "role" : role }

    def  get_appearance ( self , uid ):
        apprnc  =  self . Redis . Lrange ( f "uid: { uid } : внешний вид" , 0 , - 1 )
        если  не  apprnc :
            вернуть  Ложь
        return { "n" : apprnc [ 0 ], "nct" : int ( apprnc [ 1 ]), "g" : int ( apprnc [ 2 ]),
                "sc" : int ( apprnc [ 3 ]), "ht" : int ( apprnc [ 4 ]),
                "hc" : int ( apprnc [ 5 ]), " brt " : int ( apprnc [ 6 ]),
                "brc" : int ( apprnc [ 7 ]), "et" : int ( apprnc [ 8 ]),
                "ec" : int ( apprnc [ 9 ]), "fft" : int ( apprnc [ 10 ]),
                "fat" : int ( apprnc [ 11 ]), "fac" : int ( apprnc [ 12 ]),
                "ss" : int ( apprnc [ 13 ]), "ssc" : int ( apprnc [ 14 ]),
                "mt" : int ( apprnc [ 15 ]), "mc" : int ( apprnc [ 16 ]),
                "sh" : int ( apprnc [ 17 ]), "shc" : int ( apprnc [ 18 ]),
                "rg" : int ( apprnc [ 19 ]), "rc" : int ( apprnc [ 20 ]),
                «pt» : int ( apprnc [ 21 ]), «pc» : int ( apprnc [ 22 ]),
                "bt" : int ( apprnc [ 23 ]), "bc" : int ( apprnc [ 24 ])}

    def  get_clothes ( self , uid , type_ ):
        одежда  = []
        за  предмет  в  себе . Redis . smembers ( f "uid: { uid } : wear " ):
            если  "_"  в  элементе :
                id_ , clid  =  item . разделить ( "_" )
                одежда . append ({ "id" : id_ , "clid" : clid })
            еще :
                одежда . append ({ "id" : item , "clid" : нет })
        если  type_  ==  1 :
            clths  = { "cc" : "casual" , "ccltns" : { "casual" : { "cct" : [],
                                                           "cn" : "" ,
                                                           "ctp" : "casual" }}}
            за  предмет  в  одежде :
                if  item [ "clid" ]:
                    clths [ "ccltns" ] [ "casual" ] [ "cct" ]. append ( f " { item [ 'id' ] } :"
                                                            f " { item [ 'clid' ] } " )
                еще :
                    clths [ "ccltns" ] [ "casual" ] [ "cct" ]. добавить ( item [ "id" ])
        elif  type_  ==  2 :
            clths  = { "clths" : []}
            за  предмет  в  одежде :
                clths [ "clths" ]. append ({ "tpid" : item [ "id" ],
                                       "clid" : item [ "clid" ]})
        elif  type_  ==  3 :
            clths  = { "cct" : []}
            за  предмет  в  одежде :
                if  item [ "clid" ]:
                    clths [ "cct" ]. append ( f " { item [ 'id' ] } : { item [ 'clid' ] } " )
                еще :
                    clths [ "cct" ]. добавить ( item [ "id" ])
        возвратные  слова

    def  get_room_items ( self , uid , room ):
        если  "_"  в  комнате :
            поднять  исключения . WrongRoom ()
        элементы  = []
        для  имени  в  себе . Redis . smembers ( f "rooms: { uid } : { room } : items" ):
            пункт  =  себя . Redis . lrange ( f "rooms: { uid } : { room } : items: { name } " , 0 , - 1 )
            имя , крышка  =  имя . разделить ( "_" )
            если  лен ( вещь ) ==  5 :
                предметы . append ({ "tpid" : name , "x" : float ( item [ 0 ]),
                              "y" : float ( элемент [ 1 ]), "z" : float ( item [ 2 ]),
                              "d" : int ( элемент [ 3 ]), "lid" : int ( lid ),
                              "избавить" : элемент [ 4 ]})
            еще :
                предметы . append ({ "tpid" : name , "x" : float ( item [ 0 ]),
                              "y" : float ( элемент [ 1 ]), "z" : float ( item [ 2 ]),
                              "d" : int ( item [ 3 ]), "lid" : int ( lid )})
        вернуть  предметы

    def  _background ( self ):
        пока  верно :
            каротаж . info ( f "Игроки онлайн: { len ( self . online ) } " )
            для  UID  в  себе . инв . copy ():
                inv  =  self . inv [ uid ]
                если  инв . истекает  и  время . время () -  инв . истекает  >  0 :
                    дель  самоуправления . inv [ uid ]
            время . сон ( 60 )


if  __name__  ==  "__main__" :
    каротаж . basicConfig ( format = "% (имя уровня) -8s [% (asctime) s]% (сообщение) s" ,
                        datefmt = "% H:% M:% S" , уровень = регистрация . ОТЛАДКА )
    сервер  =  сервер ()
    сервер . слушать ()
<? xml version = " 1.0 " ?>

< config >

    < профиль > местный </ профиль >

    < collectStats > false </ collectStats >
    < bigBrother  url = " / big_brother "  project = " local "  active = " false " />
    < loadingStatisticUrl > / loadingStats </ loadingStatisticUrl >
    < statsActive > false </ statsActive >
    < loginCallback > / prelogin </ loginCallback >
    < applicationId > avataria-local </ applicationId >
    < production > true </ production >

    < serverconfig  protocol = " binary "  encrypted = " false "  сжатый = " false "  checkumed = " true " />
    < hardVirality > false </ hardVirality >

    < locationRestrictions >
        <! - <item id = "canyon" on = "true" allowIds = "" layer2Active = "true" /> ->
        <! - school = yandexSchool, schoolAvataria = обычная школа. Посмотреть больше идентификаторов в WorkScenarioTypes.as ->
        < item  id = " school "  on = " false "  allowIds = " " />
        < item  id = " schoolAvataria "  on = " false "  allowIds = " " />
    </ locationRestrictions >

    < startUpDialog >
        < welcome  popup = " false "  button = " false "  version = " 18 " />
        < sharedObjectAward  popup = " true " />
    </ startUpDialog >

    < globalNews  active = " true " />

    < клан >
        < logoUrl > {{address}} / files / promo / clan / logo / {$ id} .png </ logoUrl >
    </ clan >

    < contentConfigs >
        < ContentConfig  CDN = " по умолчанию " >
            < contentUrl > {{address}} / files / </ contentUrl >
            < contentMirrorUrl > </ contentMirrorUrl >
            < furnitureUrl > {{address}} / files / swf / furniture / </ furnitureUrl >
            < furnitureMirrorUrl > </ furnitureMirrorUrl >
        </ contentConfig >
    </ contentConfigs >

    < clientLogUrl > / client_log </ clientLogUrl >

    < supportUrl > https://github.com/AvaCity/avacity-2.0/issues </ supportUrl >

    < island  show = " true "  exchange = " true " />
    < rating  on = " true "  allowIds = " " />
    < offerAllowed > false </ offerAllowed >
    < activityRating  on = " false " />

    < AllowedIds > </ allowedIds >

    < preloaderThemes >
        < theme  id = " pillows "  url = " {{address}} / files / swf / preloader / PillowFightComix_ru.swf " />
        < theme  id = " romance "  url = " {{address}} / files / swf / preloader / RomanceComix_ru.swf " />
        < theme  id = " dress "  url = " {{address}} / files / swf / preloader / GirlsDressesComix_ru.swf " />
    </ preloaderThemes >

    < appNotAvailableUrls  bgUrl = " {{address}} / files / promo / error / ErrorMessageBg.png " >
        < message > {{address}} / files / promo / error / ErrorMessage1_ru.png </ message >
        < message > {{address}} / files / promo / error / ErrorMessage2_ru.png </ message >
        < message > {{address}} / files / promo / error / ErrorMessage3_ru.png </ message >
    </ appNotAvailableUrls >

    < социальный >
        < application  name = " city " >
            < apiUrl > / social </ apiUrl >
            < paymentCallback > / localCallback </ paymentCallback >
            < userId > {{uid}} </ userId >
            < пароль > {{токен}} </ пароль >
        </ application >
    </ social >
    
    < adminServer > {{ip}} </ adminServer >
    < adminPort > 8123 </ adminPort >

    < outsideServer > {{ip}} </ outsideServer >
    < outsidePort > 8123 </ outsidePort >

    < houseServer > {{ip}} </ houseServer >
    < housePort > 8123 </ housePort >

    < workServer > {{ip}} </ workServer >
    < WorkPort > 8123 </ workPort >

    < islandServer > {{ip}} </ islandServer >
    < islandPort > 8123 </ islandPort >
    
    < resourseMap >
        < avatarIcons > swf / AvatarIcons.swf </ avatarIcons >
        < AppearanceIcons > швейцарских франков / AppearanceIcons.swf </ appearanceIcons >
        < avatarAnimation > swf / AvatarAnimation.swf </ avatarAnimation >
        < avatarAppearance > swf / AvatarAppearance.swf </ avatarAppearance >
        < boyAvatarClothes > swf / BoyAvatarClothes.swf </ boyAvatarClothes >
        < girlAvatarClothes > swf / GirlAvatarClothes.swf </ girlAvatarClothes >

        < ui > swf / Ui.swf </ ui >
        < uiInit > swf / UiInit.swf </ uiInit >
        < uiLang > swf / UiLang_: lang: .swf </ uiLang >
        < location > swf / Location.swf </ location >

        < house > swf / House.swf </ house >
        < kitchen > swf / Kitchen.swf </ kitchen >
        < bathroom > swf / Bathroom.swf </ bathroom >
        < furniture > swf / Furniture.swf </ furniture >
        < decor > swf / Decor.swf </ decor >

        < yard > swf / Yard.swf </ yard >
        < vip > swf / Vip.swf </ vip >
        < cafe > swf / Cafe.swf </ cafe >
        < club > swf / Club.swf </ club >
        < street > swf / Street.swf </ street >
        < park > swf / Park.swf </ park >
        < garden > swf / Garden.swf </ garden >
        < fortune > swf / FortuneGame_: lang: .swf </ fortune >
        < sounds > swf / Sounds.swf </ sounds >
        < terrain > swf / Terrain.swf </ terrain >
        < landscape > swf / Landscape.swf </ landscape >
        < salon > swf / Salon.swf </ salon >
        < school > swf / School.swf </ school >
        < schoolUi > swf / YandexUi.swf </ schoolUi >
        < photoSalon > swf / PhotoSalon.swf </ photoSalon >
        < photoSalonPro > swf / PhotoSalonPro.swf </ photoSalonPro >
        < publicBeach > swf / PublicBeach.swf </ publicBeach >
        < canyon > swf / Canyon.swf </ canyon >
        < ballroom > swf / Ballroom.swf </ ballroom >
        < couturier > swf / Couturier.swf </ couturier >
        < Предложения > швейцарских франков / Offers.swf </ предложения >
        < weddingBeach > swf / WeddingBeach.swf </ weddingBeach >
        < iceRink > swf / IceRink.swf </ iceRink >
        < wallPost > swf / WallPost.swf </ wallPost >
        < mobile > swf / Mobile.swf </ mobile >
        < clan > swf / Clan.swf </ clan >
        < clanHall > swf / ClanHall.swf </ clanHall >

        < newyear2014 > swf / modules / NewYear2014.swf </ newyear2014 >
        < stval2014 > swf / modules / StValentine2014.swf </ stval2014 >
        < dzo2014 > swf / modules / Dzo2014.swf </ dzo2014 >
        < springDay2014 > swf / modules / SmrDay2014.swf </ springDay2014 >
        < steampunk2014 > swf / modules / Steampunk2014.swf </ steampunk2014 >
        < newyear2015 > swf / modules / NewYear2015.swf </ newyear2015 >
        < stval2015 > swf / modules / StValentine2015.swf </ stval2015 >
        < springDay2015 > swf / modules / SpringDay2015.swf </ springDay2015 >
        < may1st2015 > swf / modules / May1st2015.swf </ may1st2015 >
        < dirol2015 > swf / modules / Dirol2015.swf </ dirol2015 >
        < kotex2015 > swf / modules / Kotex2015.swf </ kotex2015 >
        < newyear2016 > swf / modules / NewYear2016.swf </ newyear2016 >
        < springDay2016 > swf / modules / SpringDay2016.swf </ springDay2016 >
        < firstApril2016 > swf / modules / FirstApril2016.swf </ firstApril2016 >
        < avaBirthday2016 > swf / modules / AvaBirthday2016.swf </ avaBirthday2016 >
        < familyDay2016 > swf / modules / FamilyDay2016.swf </ familyDay2016 >
        < halloween2016 > swf / modules / Halloween2016.swf </ halloween2016 >
        < newyear2017 > swf / modules / NewYear2017.swf </ newyear2017 >
        < shrovetide2017 > swf / modules / Shrovetide2017.swf </ shrovetide2017 >
        < springDay2017 > swf / modules / SpringDay2017.swf </ springDay2017 >
        < firstApril2017 > swf / modules / FirstApril2017.swf </ firstApril2017 >
        < avaBirthday2017 > swf / modules / AvaBirthday2017.swf </ avaBirthday2017 >
        < halloween2017 > swf / modules / Halloween2017.swf </ halloween2017 >
        < clubHlwn > swf / events / ClubHlwn.swf </ clubHlwn >
        < locationHlwn > swf / events / LocationHlwn.swf </ locationHlwn >
        < newyear2018 > swf / modules / NewYear2018.swf </ newyear2018 >
        < skiResort > swf / SkiResort.swf </ skiResort >
        < cafeNY > swf / events / CafeNY.swf </ cafeNY >
        < streetNY > swf / events / StreetNY.swf </ streetNY >
        < clubNY > swf / events / ClubNY.swf </ clubNY >
        < landscapeNY > swf / events / LandscapeNY.swf </ landscapeNY >
        < parkNY > swf / events / ParkNY.swf </ parkNY >
        < salonNY > swf / events / SalonNY.swf </ salonNY >
        < vipNY > swf / events / VipNY.swf </ vipNY >
        < stVal2018 > swf / modules / StValentine2018.swf </ stVal2018 >
        < dzo2018 > swf / modules / Dzo2018.swf </ dzo2018 >
        < springDay2018 > swf / modules / SpringDay2018.swf </ springDay2018 >
        < firstApril2018 > swf / modules / FirstApril2018.swf </ firstApril2018 >
        < avaBirthday2018 > swf / modules / AvaBirthday2018.swf </ avaBirthday2018 >
        < halloween2018 > swf / modules / Halloween2018.swf </ halloween2018 >
        < воспоминания > SWF / Memories.swf </ воспоминания >
        < parkHlwn > swf / events / ParkHlwn.swf </ parkHlwn >
        < podium > swf / Podium.swf </ podium >
        < newyear2019 > swf / modules / NewYear2019.swf </ newyear2019 >
        < vkPay2018 > swf / modules / VKPay2018.swf </ vkPay2018 >
        < stval2019 > swf / modules / StValentine2019.swf </ stval2019 >
        < dzo2019 > swf / modules / Dzo2019.swf </ dzo2019 >
        < springDay2019 > swf / modules / SpringDay2019.swf </ springDay2019 >
        < firstApril2019 > swf / modules / FirstApril2019.swf </ firstApril2019 >
        < streetAvaBD > swf / events / StreetAvaBD.swf </ streetAvaBD >
        < seasonEvent > swf / modules / SeasonEvent.swf </ seasonEvent >

        < config > data / config_all_ru.zip </ config >
    </ resourseMap >
    
    < resourseMapMirror >
        < avatarIcons > swf / AvatarIcons.swf </ avatarIcons >
        < AppearanceIcons > швейцарских франков / AppearanceIcons.swf </ appearanceIcons >
        < avatarAnimation > swf / AvatarAnimation.swf </ avatarAnimation >
        < avatarAppearance > swf / AvatarAppearance.swf </ avatarAppearance >
        < boyAvatarClothes > swf / BoyAvatarClothes.swf </ boyAvatarClothes >
        < girlAvatarClothes > swf / GirlAvatarClothes.swf </ girlAvatarClothes >

        < ui > swf / Ui.swf </ ui >
        < uiInit > swf / UiInit.swf </ uiInit >
        < uiLang > swf / UiLang_: lang: .swf </ uiLang >
        < location > swf / Location.swf </ location >

        < house > swf / House.swf </ house >
        < kitchen > swf / Kitchen.swf </ kitchen >
        < bathroom > swf / Bathroom.swf </ bathroom >
        < furniture > swf / Furniture.swf </ furniture >
        < decor > swf / Decor.swf </ decor >

        < yard > swf / Yard.swf </ yard >
        < vip > swf / Vip.swf </ vip >
        < cafe > swf / Cafe.swf </ cafe >
        < club > swf / Club.swf </ club >
        < street > swf / Street.swf </ street >
        < park > swf / Park.swf </ park >
        < garden > swf / Garden.swf </ garden >
        < fortune > swf / FortuneGame_: lang: .swf </ fortune >
        < sounds > swf / Sounds.swf </ sounds >
        < terrain > swf / Terrain.swf </ terrain >
        < landscape > swf / Landscape.swf </ landscape >
        < salon > swf / Salon.swf </ salon >
        < school > swf / School.swf </ school >
        < schoolUi > swf / YandexUi.swf </ schoolUi >
        < photoSalon > swf / PhotoSalon.swf </ photoSalon >
        < photoSalonPro > swf / PhotoSalonPro.swf </ photoSalonPro >
        < publicBeach > swf / PublicBeach.swf </ publicBeach >
        < canyon > swf / Canyon.swf </ canyon >
        < Предложения > швейцарских франков / Offers.swf </ предложения >
        < weddingBeach > swf / WeddingBeach.swf </ weddingBeach >
        < iceRink > swf / IceRink.swf </ iceRink >
        < wallPost > swf / WallPost.swf </ wallPost >
        < mobile > swf / Mobile.swf </ mobile >
        < clan > swf / Clan.swf </ clan >
        < clanHall > swf / ClanHall.swf </ clanHall >

        < newyear2014 > swf / modules / NewYear2014.swf </ newyear2014 >
        < stval2014 > swf / modules / StValentine2014.swf </ stval2014 >
        < dzo2014 > swf / modules / Dzo2014.swf </ dzo2014 >
        < springDay2014 > swf / modules / SmrDay2014.swf </ springDay2014 >
        < steampunk2014 > swf / modules / Steampunk2014.swf </ steampunk2014 >
        < newyear2015 > swf / modules / NewYear2015.swf </ newyear2015 >
        < stval2015 > swf / modules / StValentine2015.swf </ stval2015 >
        < may1st2015 > swf / modules / May1st2015.swf </ may1st2015 >
        < dirol2015 > swf / modules / Dirol2015.swf </ dirol2015 >
        < kotex2015 > swf / modules / Kotex2015.swf </ kotex2015 >
        < newyear2016 > swf / modules / NewYear2016.swf </ newyear2016 >
        < springDay2016 > swf / modules / SpringDay2016.swf </ springDay2016 >
        < firstApril2016 > swf / modules / FirstApril2016.swf </ firstApril2016 >
        < avaBirthday2016 > swf / modules / AvaBirthday2016.swf </ avaBirthday2016 >
        < familyDay2016 > swf / modules / FamilyDay2016.swf </ familyDay2016 >
        < halloween2016 > swf / modules / Halloween2016.swf </ halloween2016 >
        < newyear2017 > swf / modules / NewYear2017.swf </ newyear2017 >
        < shrovetide2017 > swf / modules / Shrovetide2017.swf </ shrovetide2017 >
        < springDay2017 > swf / modules / SpringDay2017.swf </ springDay2017 >
        < firstApril2017 > swf / modules / FirstApril2017.swf </ firstApril2017 >
        < avaBirthday2017 > swf / modules / AvaBirthday2017.swf </ avaBirthday2017 >
        < halloween2017 > swf / modules / Halloween2017.swf </ halloween2017 >
        < clubHlwn > swf / events / ClubHlwn.swf </ clubHlwn >
        < locationHlwn > swf / events / LocationHlwn.swf </ locationHlwn >
        < newyear2018 > swf / modules / NewYear2018.swf </ newyear2018 >
        < skiResort > swf / SkiResort.swf </ skiResort >
        < cafeNY > swf / events / CafeNY.swf </ cafeNY >
        < streetNY > swf / events / StreetNY.swf </ streetNY >
        < clubNY > swf / events / ClubNY.swf </ clubNY >
        < landscapeNY > swf / events / LandscapeNY.swf </ landscapeNY >
        < parkNY > swf / events / ParkNY.swf </ parkNY >
        < salonNY > swf / events / SalonNY.swf </ salonNY >
        < vipNY > swf / events / VipNY.swf </ vipNY >
        < stVal2018 > swf / modules / StValentine2018.swf </ stVal2018 >
        < dzo2018 > swf / modules / Dzo2018.swf </ dzo2018 >
        < springDay2018 > swf / modules / SpringDay2018.swf </ springDay2018 >
        < firstApril2018 > swf / modules / FirstApril2018.swf </ firstApril2018 >
        < avaBirthday2018 > swf / modules / AvaBirthday2018.swf </ avaBirthday2018 >
        < halloween2018 > swf / modules / Halloween2018.swf </ halloween2018 >
        < воспоминания > SWF / Memories.swf </ воспоминания >
        < parkHlwn > swf / events / ParkHlwn.swf </ parkHlwn >
        < podium > swf / Podium.swf </ podium >
        < newyear2019 > swf / modules / NewYear2019.swf </ newyear2019 >
        < vkPay2018 > swf / modules / VKPay2018.swf </ vkPay2018 >
        < stval2019 > swf / modules / StValentine2019.swf </ stval2019 >
        < dzo2019 > swf / modules / Dzo2019.swf </ dzo2019 >
        < springDay2019 > swf / modules / SpringDay2019.swf </ springDay2019 >
        < firstApril2019 > swf / modules / FirstApril2019.swf </ firstApril2019 >
        < streetAvaBD > swf / events / StreetAvaBD.swf </ streetAvaBD >
        < seasonEvent > swf / modules / SeasonEvent.swf </ seasonEvent >

        < config > data / config_all_ru.zip </ config >
    </ resourseMapMirror >

    < musicTracks >
        < cafe1 > music / Cafe1.mp3 </ cafe1 >
        < cafe2 > music / Cafe2.mp3 </ cafe2 >
        < home1 > music / Home1.mp3 </ home1 >
        < home2 > music / Home2.mp3 </ home2 >
        < job1 > music / Job1.mp3 </ job1 >
        < job2 > music / Job2.mp3 </ job2 >
        < outside1 > music / Outside1.mp3 </ outside1 >
        < outside2 > music / Outside2.mp3 </ outside2 >
        < vip1 > music / VIP1.mp3 </ vip1 >
        < vip2 > music / VIP2.mp3 </ vip2 >
        < club1 > music / Club1.mp3 </ club1 >
        < club2 > music / Club2.mp3 </ club2 >
        < skiResort > music / SkiResort.mp3 </ skiResort >
        < testTrack1 > music / TopForTest.mp3 </ testTrack1 >
        < testTrack2 > music / CrossLilu.mp3 </ testTrack2 >
    </ musicTracks >

</ config >
<! DOCTYPE html >
< html >
< голова >
    < meta  charset = " utf-8 " >
    < title > Ava.City 2.0 </ title >
</ head >
< тело >
    {% если не токен%}
    < form  method = " post " action = " / login " >
        < таблица >
            < tr >
                < td > < label  for = " loginField " > Логин </ label > </ td >
                < td > < input  id = " loginField " type = " text " name = " login " требуется > </ td >
            </ tr >
            < tr >
                < td > < label  for = " passwordField " > Пароль </ label > </ td >
                < td > < input  id = " passwordField " type = " password " name = " password " требуется > </ td >
            </ tr >
            < tr >
                < td  colspan = " 2 " style = " text-align: center " > < input  type = " submit " value = " Войти " > </ td >
            </ tr >
        </ table >
    </ form >
    < Р > < HREF =» / зарегистрировать " > Зарегистрироваться </ > </ р > {% остальное%} < ввода ID =" ClickMe " Тип =" Кнопка " значение =" Начать игру " OnClick =" начать () ; "/>  
    < Стиль = " дисплей: нет; " ID =" вспышка " HREF =" https://get.adobe.com/flashplayer " > Включить вспышки </ > 
    < div  id = " flashContent " > </ div >
    < Бр >
    < HREF = " / выход из системы " > Выйти </ > 
    < скрипт >
        function  start ( ) {
            документ . getElementById ( 'clickMe' ) . стиль . display  =  "none" ;
            if  ( swfobject . getFlashPlayerVersion ( ) . major  ==  0 ) {
                документ . getElementById ( 'flash' ) . стиль . дисплей  =  "блок" ;
                возврат ;
            }
            var  vars  =  { } ;
            vars [ 'config' ]  =  '/appconfig.xml' ;
            vars [ ' version ' ]  =  '/files/versions.json? {{update_time}}' ;
            vars [ 'appSwf' ]  =  '/files/pnz-city.swf? {{update_time}}' ;
            var  flashVars  =  [ ] ;
            for  ( переменная var  в vars ) {   
                flashVars . push ( переменная  +  '='  +  vars [ переменная ] ) ;
            }
            var  params  =  {
                flashvars : flashVars . присоединиться ( "&" ) ,
                allowfullscreen : «верно» ,
                allowcriptaccess : "всегда" ,
                allowFullScreenInteractive : "true" ,
                разрешить сетевое взаимодействие : «все» ,
                wmode : "прозрачный"
            } ;
            var  el  =  документ . getElementById ( "flashContent" ) ;
            swfobject . embedSWF ( "/files/pnz-city-container.swf" ,  el ,  1280 ,  720 ,  10 , null , null ,  params ,  { name : "flashContent" } ) ;
        }
    </ script >
    < script  type = " text / javascript " src = " /files/swfobject.js " > </ script > {% endif%}
</ body >
</ html >
импорт  хешлиб
импортировать  zipfile
импорт  JSON
импорт  ОС

ос . чдир ( ".." )
z  =  zipfile . ZipFile ( "files / data / config_all_ru.zip" , mode = "w" )
для  root , dirs , файлы  в  os . walk ( "config_all_ru" ):
    для  файла  в  файлах :
        г . запись ( os . path . join ( root , file ),
                arcname = os . путь . присоединиться ( root ,
                                     файл ). split ( "config_all_ru /" ) [ 1 ])
г . закрыть ()
с  открытым ( "files / data / config_all_ru.zip" , mode = "rb" ) как  f :
    hash_  =  hashlib . md5 ( е . чтения ()). hexdigest ()
ос . переименовать ( "files / data / config_all_ru.zip" ,
          f "files / data / config_all_ru_ { hash_ } .zip" )
с  открытым ( "files / versions.json" ) как  f :
    версии  =  json . нагрузка ( е )
версии [ "data / config_all_ru.zip" ] =  хэш_
с  открытым ( "files / versions.json" , "w" ) как  f :
    ф . написать ( json . дампы ( версии ))
print ( f "Готово, файл config_all_ru_ { hash_ } .zip (уже записан в versions.json)" )
