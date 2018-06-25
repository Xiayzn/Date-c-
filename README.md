# Date-c-



class Date
{
public:
	Date(int year = 1990, int month = 1, int day = 1)
		:_year(year)
		, _month(month)
		, _day(day)
	{
		if (!(year >= 0 && (month > 0 && month < 13)
			&& (day > 0 && day <= _GetDaysofMonth(year, month))))
		{
			_year = 1990;
			_month = 1;
			_day = 1;
		}
	}

	Date operator+(int days)
	{
		if (days < 0)
			return *this - (0 - days);
		Date temp(*this);
		temp._day += days;
		int daysofMonth;
		while (temp._day > (daysofMonth = _GetDaysofMonth(temp._year, temp._month)))
		{
			temp._day -= daysofMonth;
			temp._month++;
			if (temp._month > 12)
			{
				temp._year += 1;
				temp._month = 1;
			}
		}
		return temp;
	}

	Date operator-(int days)
	{
		if (days < 0)
			return *this + (0 - days);
		Date temp(*this);
		temp._day -= days;
		while (temp._day <= 0)
		{
			temp._month--;
			if (temp._month == 0)
			{
				temp._year -= 1;
				temp._month = 12;
			}
			temp._day += -_GetDaysofMonth(temp._year, temp._month);
		}
		return temp;
	}

	Date& operator++()
	{
		*this = *this + 1;
		return *this;
	}

	Date operator++(int)
	{
		Date temp(*this);
		++(*this);
		return temp;
	}

	Date& operator--()
	{
		*this = *this - 1;
		return *this;
	}

	Date operator--(int)
	{
		Date temp(*this);
		--(*this);
		return temp;
	}
	
	bool operator<(const Date& d)
	{
		if (_year < d._year ||
			(_year == d._year && _month < d._month) ||
			(_year == d._year && _month == d._month && _day < d._day))
		{
			return true;
		}
		return false;
	}

	bool operator==(const Date& d)
	{
		return _year == d._year &&
			   _month == d._month &&
			   _day == d._day;
	}

	bool operator!=(const Date& d)
	{
		return !(*this == d);
	}

	//知道两个日期之间差多少天
	int operator-(const Date& d)
	{
		Date minDate(*this);
		Date maxDate(d);
		if (maxDate < minDate)
		{
			minDate = d;
			maxDate = *this;
		}
		size_t count = 0;
		if (minDate < maxDate)
		{
			++count;
			++minDate;
		}
		return count;
	}
private:
	bool _IsLeap(int year)
	{
		if ((0 == year % 4 && 0 != year % 100) || (0 == year % 400))
		{
			return true;
		}

		return false;
	}

	int _GetDaysofMonth(int year, int month)
	{
		int days[] = {0,31,28,31,30,31,30,31,31,30,31,30,31};
		if (2 == month && _IsLeap(year))
			days[2] += 1;
		return days[month];
	}
	
private:
	int _year;
	int _month;
	int _day;
};
