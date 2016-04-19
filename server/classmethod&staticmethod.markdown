### @classmethod, @staticmethod


- classmethod must have a reference to a class object as the first parameter
- whereas staticmethod can have no parameters at all


## Example

    class Date(object):
    
        day = 0
        month = 0
        year = 0
    
        def __init__(self, day=0, month=0, year=0):
            self.day = day
            self.month = month
            self.year = year
            


    """ cls is a container of holds class itself """
    @classmethod
    def from_string(cls, date_as_string):
        day, month, year = map(int, date_as_string.split('-'))
        date1 = cls(day, month, year)
        return date1
        

    @staticmethod
    def is_date_valid(date_as_string):
        day, month, year = map(int, date_as_string.split('-'))
        return day <= 31 and month <= 12 and year <= 3999
        



    date2 = Date.from_string('11-09-2012') # classmethod
    is_date = Date.is_date_valid('11-09-2012') # staticmethod