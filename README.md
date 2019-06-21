# Script-for-Date-Dimension-SQL-Server

## DimDate

CREATE TABLE #DimDate(
	[MemberId] int primary key NOT NULL,
	[FYPeriod] int NOT NULL,
	[FYPeriodInYear] int NOT NULL,
	[PeriodStart] datetime NOT NULL,
	[PeriodEnd] datetime NOT NULL,
	[FYYear] int NULL,
	[FYYearLabel] varchar(40) NULL,
	[FYQuarter] int NULL,
	[FYQuarterLabel] varchar(40) NULL,
	[FYMonthLabel] varchar(40) NULL,
	[FYWeek] int NULL,
	[FYWeekLabel] varchar(40) NULL,
	[Day] varchar(40) NULL,
	[CalendarYear] int NULL,
	[CalendarMonth] int NULL,
	[CalendarDay] int NULL,
	[Entity] varchar(50) NULL)

DECLARE @StartDate DATETIME = '01/01/2021' --Starting value of Date Range
DECLARE @EndDate DATETIME = '01/01/2025' --End Value of Date Range

WHILE @StartDate < @EndDate
BEGIN 
	INSERT INTO #DimDate
	SELECT
		
	CONVERT (char(8),@Startdate,112) as MemberId,
		CONVERT(VARCHAR, DATEPART(Year, @Startdate))  + RIGHT('0' + CONVERT(VARCHAR, DATEPART(Month, @Startdate)),2) AS FYPeriod,
		DATEPART(Month, @Startdate) AS FYPeriodInYear,
		DATEADD(month, DATEDIFF(month, 0, @Startdate), 0) AS PeriodStart,
		DATEADD(DAY, -1, DATEADD(MONTH, DATEDIFF(MONTH, 0, @Startdate) + 1, 0)) AS PeriodEnd,
		DATEPART(YEAR, @Startdate) AS FYYear,
		'Yr'+'-'+ convert(varchar(20),DATEPART(YEAR, @Startdate)) As FYYearLabel,
		DATEPART(quarter, @Startdate) AS Quarter,
		'Qtr'+'-'+
		convert(varchar(20),DATEPART(quarter, @Startdate)) As FYQuaterLabel,
		DATENAME(Month, @Startdate) AS FYMonthLabel,
		DATEPART(Weekday, @Startdate) AS FYWeek,
	    'WK'+'-'+ 
		convert(varchar(20),DATEPART(week, @Startdate)) AS FYWeekLabel,
		DATENAME(weekday, @Startdate) AS Day,
		DATEPART(YEAR, @Startdate) AS CalenderYear,
		DATEPART(Month, @Startdate) AS CalenderMonth,
		DATEPART(day, @Startdate) AS CalenderDay,
		'' As entity

		SET @Startdate = DATEADD(day, 1, @Startdate)
		
end
--drop table #DimDate
--truncate table #DimDate
SELECT * FROM #DimDate

