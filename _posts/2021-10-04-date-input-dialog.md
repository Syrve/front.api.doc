---
title: Date and time input windows
layout: default
---

In API V7, it became possible to show date and time request windows (see article «[Date and time prompt windows]({{ site.baseurl }}/v7/en/DateTimePopups.html)»).

#### 1. Date entry window.

The date request window is shown using the method [`DateTime? IViewManager.ShowDateNumpadPopup(DateTime selectedDate, string title)`](https://syrve.github.io/front.api.sdk/v7/html/M_Resto_Front_Api_UI_IViewManager_ShowDateNumpadPopup.htm)

![date-numpad-popup](../../../img/showDateTimePopup/DateNumpadPopup.png)

#### 2. Date and time input window.

The date and time request window is shown using the method [`DateTime? IViewManager.ShowDateTimePopup(DateTime selectedDate, string title, DateTime minDate, DateTime maxDate)`](https://syrve.github.io/front.api.sdk/v7/html/M_Resto_Front_Api_UI_IViewManager_ShowDateTimePopup.htm)

![date-time-popup](../../../img/showDateTimePopup/DateTimePopup.png)

#### 3. Calendar window.

You can display the date entry window as a calendar. A method has been added for this purpose [`DateTime? IViewManager.ShowCalendarPopup(DateTime selectedDate, string title, DateTime minDate, DateTime maxDate)`](https://syrve.github.io/front.api.sdk/v7/html/M_Resto_Front_Api_UI_IViewManager_ShowCalendarPopup.htm)

![calendar-popup](../../../img/showDateTimePopup/CalendarPopup.png)