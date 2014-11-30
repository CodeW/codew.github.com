---
layout: post
title: "use symfony"
description: ""
category: [symfony]
tags: [symfony]
---
{% include JB/setup %}

### base ###

1.	创建bundle
	
		php app/console generate:bundle --namespace=Acme/StoreBundle --format=yml
		
2.	创建Route

		sudo vim src/Acme/StoreBundle/Resources/config/routing.yml
		# 修改后的内容
		acme_store_homepage:
		    path:     /createproduct
    		defaults: { _controller: AcmeStoreBundle:Default:create }
		
3.	创建Controller

		sudo vim src/Acme/StoreBundle/Controller/DefaultController.php
		# 修改后的内容
		<?php

        namespace Acme\StoreBundle\Controller;

        use Symfony\Bundle\FrameworkBundle\Controller\Controller;
        use Acme\StoreBundle\Entity\Product;
        use Symfony\Component\HttpFoundation\Response;

        class DefaultController extends Controller
        {
            public function indexAction($name)
            {
                return $this->render('AcmeStoreBundle:Default:index.html.twig', array('name' => $name));
            }
            public function createAction()
            {
                $product = new Product();
                $product->setName('A Foo Bar');
                $product->setPrice('19.99');
                $product->setDescription('Lorem ipsum dolor');

                $em = $this->getDoctrine()->getEntityManager();
                $em->persist($product);
                $em->flush();

                return new Response('Created product id '.$product->getId());
            }
        }
		
### database ###

1.	添加数据

		// src/Acme/StoreBundle/Controller/DefaultController.php
        use Acme\StoreBundle\Entity\Product;
        use Symfony\Component\HttpFoundation\Response;
        // ...

        public function createAction()
        {
            $product = new Product();
            $product->setName('A Foo Bar');
            $product->setPrice('19.99');
            $product->setDescription('Lorem ipsum dolor');

            $em = $this->getDoctrine()->getEntityManager();
            $em->persist($product);
            $em->flush();

            return new Response('Created product id '.$product->getId());
        }	
