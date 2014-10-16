---
layout: post
title: "BinSearch"
description: ""
category: [algorithms]
tags: [algorithm, search]
---
{% include JB/setup %}

	<?php
	class BinSearch {
		/* 几个要点：
		 * 1. high=n-1 => while(low <= high) => high=middle-1;    
		 * 2. high=n   => while(low <  high) => high=middle;    
		 * 3. middle的计算注意不要写在while循环外，否则其不能被更新了。
		 */
		public static function bs1($arr, $search) {
			if (empty($arr)) {
				return;
			}

			$low = 0;
			$high = count($arr) - 1;
			while ($low <= $high) {
				$middle = $low + (int) (($high - $low) / 2);
				if ($arr[$middle] > $search) {
					$high = $middle - 1;
				} elseif ($arr[$middle] < $search) {
					$low = $middle + 1;
				} else {
					return $middle;
				}
			}

			return -1;
		}
	}

	$arr = range(1, 10);
	shuffle($arr);
	array_splice($arr, rand(0, 7), 2, array(rand(1, 10), rand(1, 10)));
	sort($arr);
	$search = $arr[rand(0, 9)] + 1;

	echo "arr\n";
	print_r($arr);
	echo "\n";
	echo "search\n";
	print_r($search);
	echo "\n";
	echo "pos\n";
	print_r(BinSearch::bs1($arr, $search));