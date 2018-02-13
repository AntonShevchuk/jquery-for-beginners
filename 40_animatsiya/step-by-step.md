## Step-by-step {#step-by-step}

Хотелось бы отдельно остановиться на функции «step», и для наглядности приведу пример реализации подобной функции, которая отображает текущее значение анимированного параметра:

var customStep = function(now, obj) {

obj.elem; // объект анимации

obj.prop; // параметр, который анимируется

obj.start; // начальное значение

obj.end; // конечное значение

obj.pos; // коэффициент, изменяется от 0 до 1

obj.options; // опции настроек анимации

_now_; // текущее значение анимированного параметра, вычисляется как

// now = (obj.end - obj.start) * obj.pos

_$(this).html(obj.prop +': '+now+obj.unit);_ // вывод текста

}

$("#box").animate({height: "+=10px"}, {step: customStep});

_Мне ни разу не приходилось использовать step-функции, лишь только для_ [_примера_](http://anton.shevchuk.name/book/code/animate.step.html)