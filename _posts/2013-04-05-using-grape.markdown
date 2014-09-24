---
layout: post
title: "Использование Grape для реализации API в вашем проекте"
date: 2013-04-04 12:00
comments: false
categories:
---

В данной статье речь пойдет о том, как можно эффективно и с минимальными затратами времени и седых волос реализовать JSON или XML API в вашем проекте. Речь пойдет об использовании гема Grape.

<!--more-->

Итак, зачем вам может понадобиться отдельная библиотека для релизации API? А она нужна вам в следующих случаях:

* Вам необходимо сложное API.
* Вы хотите отделить реализацию API от кода вашего основного приложения.
* Вам необходима поддержка разных версий API одновременно для обратной совместимости.
* Вам необходима валидация параметров запросов в ваших API.
* Вы хотите гибко управлять обработкой ошибок.
* Вы хотите сделать асинхронное API.

Если одино из утверждений в списке выше — истинно, то вам следует обратить внимание на библиотеку Grape. Если кратко, то Grape — фреймворк для быстрого создания REST-совместимого API. Он разработан для того, чтобы запускаться поверх стека Rack как отдельное приложение или дополнять существующие веб фреймоворки (Rails, Sinatra).

Grape предоставляет простой DSL для разработки RESTful API со встроенной поддержкой стандартных соглашений, включая несколько форматов, управление префиксами, валидацию контента, версионирование и многое другое.

Взгляните на пример из официальной документации Grape:

~~~ruby
module Twitter
  class API < Grape::API

    version 'v1', :using => :header, :vendor => 'twitter'
    format :json

    helpers do
      def current_user
        @current_user ||= User.authorize!(env)
      end

      def authenticate!
        error!('401 Unauthorized', 401) unless current_user
      end
    end

    resource :statuses do

      desc "Возвращаем публичный timeline."
      get :public_timeline do
        Status.limit(20)
      end

      desc "Возвращаем персональный timeline."
      get :home_timeline do
        authenticate!
        current_user.statuses.limit(20)
      end

      desc "Возвращаем статус."
      params do
        requires :id, :type => Integer, :desc => "Идентификатор статуса."
      end
      get ':id' do
        Status.find(params[:id])
      end

      desc "Создаем статус."
      params do
        requires :status, :type => String, :desc => "Ваш статус."
      end
      post do
        authenticate!
        Status.create!({
          :user => current_user,
          :text => params[:status]
        })
      end

      desc "Изменяем статус."
      params do
        requires :id, :type => String, :desc => "Идентификатор статуса."
        requires :status, :type => String, :desc => "Ваш статус."
      end
      put ':id' do
        authenticate!
        current_user.statuses.find(params[:id]).update({
          :user => current_user,
          :text => params[:status]
        })
      end

      desc "Удаляем статус."
      params do
        requires :id, :type => String, :desc => "Идентификатор статуса."
      end
      delete ':id' do
        authenticate!
        current_user.statuses.find(params[:id]).destroy
      end

    end
  end
end
~~~

Приложение будет отвечать по следующим марштурам:

    GET /statuses/public_timeline(.json)
    GET /statuses/home_timeline(.json)
    GET /statuses/:id(.json)
    POST /statuses(.json)
    PUT /statuses/:id(.json)
    DELETE /statuses/:id(.json)

Удобно, не правда ли? При помощи данного фреймворка вы получаете все необходимое для создания качественного API для вашего приложения.

Для дальнейшего ознакомления предлагаю вам дополнительные материалы:

1. [Официальная документация по Grape (англ.)](https://github.com/intridea/grape)

1. [Перевод официальной документации на сайте wtfdoc.ru](http://wtfdoc.ru/projects/grape)
