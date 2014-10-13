---
layout: post
title: "BubbleSort"
description: ""
category: [algorithms]
tags: [algorithm, sort]
---
{% include JB/setup %}

	<?php
	class BubbleSort {
		// 这一种感觉理解起来不是那么清晰
		public static function bs1(&$arr) {
			$len = count($arr);
			for ($i=0; $i < $len - 1; $i++) {
				$swap = false;
				for ($j=1; $j < $len - $i; $j++) {
					if ($arr[$j-1] > $arr[$j]) {
						$tmp = $arr[$j-1];
						$arr[$j-1] = $arr[$j];
						$arr[$j] = $tmp;
						$swap = true;
					}
				}
				if (!$swap) {
					break;
				}
			}
			return true;
		}

		// 这样相对来说就好理解多了不是
		public static function bs2(&$arr) {
			$len = count($arr);
			$swap = true;
			while ($swap) {
				$swap = false;
				for ($j=1; $j < $len; $j++) { 
					if ($arr[$j-1] > $arr[$j]) {
						$tmp = $arr[$j-1];
						$arr[$j-1] = $arr[$j];
						$arr[$j] = $tmp;
						$swap = true;
					}
				}
				$len--;
			}
			return true;
		}

		// 不加swap的判断也可以
		public static function bs3(&$arr) {
			$len = count($arr);
			while ($len > 1) {
				for ($j=0; $j < $len; $j++) { 
					if ($arr[$j-1] > $arr[$j]) {
						$tmp = $arr[$j-1];
						$arr[$j-1] = $arr[$j];
						$arr[$j] = $tmp;
					}		
				}
				$len--;
			}
			return true;
		}

		/* 进一步的优化
		 * 如果有100个数的数组，仅前面10个无序，后面90个都已排好序且都大于前面10个数字。
		 * 那么在第一趟遍历后，最后发生交换的位置必定小于10，且这个位置之后的数据必定已经有序了，
		 * 记录下这个位置。第二次只要从数组头部遍历到这个位置就可以了。当然如果未发生交换，直接结束。
		 */
		public static function bs4(&$arr) {
			$len = count($arr);
			while ($len > 1) {
				$flag = 0;
				for ($j=0; $j < $len; $j++) { 
					if ($arr[$j-1] > $arr[$j]) {
						$tmp = $arr[$j-1];
						$arr[$j-1] = $arr[$j];
						$arr[$j] = $tmp;
						$flag = $j;
					}		
				}
				$len = $flag;
			}
		}
	}

	$arr = range(1, 10);
	shuffle($arr);
	array_splice($arr, rand(0, 7), 2, array(rand(1, 10), rand(1, 10)));

	echo "before\n";
	print_r($arr);
	echo "\n";
	echo "after\n";
	BubbleSort::bs4($arr, 0, count($arr)-1);
	print_r($arr);