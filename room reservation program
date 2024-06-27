import pandas as pd
import datetime
import heapq

#Code created by team members
#explanation
#Create and save classroom and student information as a CSV file and then load it.
#The classroom information contains information on class times from Monday to Friday for 30 classrooms, which can be converted into bitmaps of 0 and 1.

building_index_map = {'33': gyungyung, '90': gukjae,
                      '32': gyungjae, '61': susun,
                      '50': hoam, '31': inmun}

gyungyung_rooms = {'33210': 0, '33212': 1, '33218': 2,
                   '33301': 3, '33302': 4, '33303': 5,
                   '33304': 6, '33305': 7, '33306': 8}
gukjae_rooms = {'90316': 0, '90318': 1}
gyungjae_rooms = {'32416': 0, '32535': 1, '32536': 2}
susun_rooms = {'61602': 0, '61603': 1, '61604': 2,
               '61605': 3, '61606': 4, '61607': 5,
               '61703': 6, '61705': 7, '61706': 8, '61707': 9}
hoam_rooms = {'50104': 0, '50105': 1, '50106': 2,
              '50304': 3, '50305': 4, '50306': 5,
              '50318': 6, '50319': 7, '50401': 8,
              '50402': 9, '50408': 10}
inmun_rooms = {'31301': 0, '31302': 1, '31307': 2,
               '31403': 3, '31405': 4, '31408': 5,
               '31501': 6, '31602': 7, '31604': 8,
               '31701': 9}

room_index_map = {
    '33': gyungyung_rooms,
    '90': gukjae_rooms,
    '32': gyungjae_rooms,
    '61': susun_rooms,
    '50': hoam_rooms,
    '31': inmun_rooms
}


all_reservation = []

#Based on the reservation end time, the queue is sorted in order of earliest end time, and then the queue is made to exit.
def add_reservation(reservation):
  heapq.heappush(all_reservation, (float(reservation[4]), float(reservation[6]), reservation))

def pop_reservation():
  if all_reservation:
    heapq.heappop(all_reservation)


# Reservations can only be made once per person
possible_num = dict(zip(student_df['Id'], student_df['Possible']))


#I need code to check if the reservation is valid, so I implemented it as a function.

def CheckReservation(Id, name, people, building, num, yoil, start_time, end_time):
    start_index = time_index_map[float(start_time)]
    end_index = time_index_map[float(end_time)]

    if Id not in student_df['Id'].values:
        print('예약자 오류')
        return False

    if lectureroom_df['Room_num'].size > 0 and lectureroom_df.loc[lectureroom_df['Room_num'] == num, 'max_mem'].values.size > 0:
        if lectureroom_df.loc[lectureroom_df['Room_num']== num, 'max_mem'].values < people:
            print('수용 인원 초과')
            return False
        elif lectureroom_df.loc[lectureroom_df['Room_num']== num, 'min_mem'].values > people:
            print('수용 인원 미만')
            return False

    if possible_num[Id] == 0:
        print('예약 횟수 초과')
        return False

    for i in range(start_index, end_index):
        if building[yoil][num][i] == 1:
            print('예약이 불가능한 시간입니다.')
            return False
        building[yoil][num][i] = 1
    return True


#Create Reservation

def Create():
    reservation_info = input("예약 정보 입력(예: 학번,이름,인원,강의실번호,예약일(YYYYMMDD),예약 시작 시간,예약 종료 시간): ").split(',')
    try:
      reserve_id = int(reservation_info[0])
      reserve_name = reservation_info[1]
      reserve_people = int(reservation_info[2])
      reserve_room = reservation_info[3]
      reserve_building_code = reserve_room[:2]
      reserve_building = building_index_map[reserve_building_code]
      reserve_room_index = room_index_map[reserve_building_code][reserve_room]

      reserve_date = reservation_info[4]
      reserve_yoil = datetime.datetime.strptime(reserve_date, '%Y%m%d')
      yoil_num = reserve_yoil.weekday()

      reserve_start_time = reservation_info[5]
      reserve_end_time = reservation_info[6]
    except:
      print('잘못된 입력입니다.')
      return
    if CheckReservation(reserve_id, reserve_name, reserve_people, reserve_building, reserve_room_index, yoil_num, reserve_start_time, reserve_end_time):
        add_reservation(reservation_info)
        no = possible_num[reserve_id]
        new_no = no-1
        possible_num[reserve_id] = new_no
        print('예약 완료')
        print(all_reservation)
        print(possible_num)

    else:
        print('예약 실패')


#When the user wants to change the reservation

def Update():
    search = input('학번을 입력하세요: ')
    for i in range(len(all_reservation)):
        search_id = all_reservation[i][2][0]
        if search_id == search:
            del_list = all_reservation.pop(i)[2]
            heapq.heapify(all_reservation) #다시 힙큐상태로 만듦
            id_num = int(del_list[0])
            room = del_list[3]
            building_code = room[:2]
            building = building_index_map[building_code]
            num = room_index_map[building_code][room]
            date = del_list[4]
            yoil = datetime.datetime.strptime(date, '%Y%m%d')
            yoil_num = yoil.weekday()
            start = del_list[5]
            end = del_list[6]

            start_time = time_index_map[float(start)]
            end_time = time_index_map[float(end)]

            for i in range(start_time, end_time):
                building[yoil_num][num][i] = 0

            no = possible_num[id_num]
            new_no = no+1
            possible_num[id_num] = new_no
            print('기존 예약 정보가 삭제되었습니다.')
            break
    else:
        print('예약 정보가 없습니다.')
        return
    update_info = Create()
