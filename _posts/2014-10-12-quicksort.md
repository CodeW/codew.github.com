---
layout: post
title: "QuickSort"
description: ""
category: [algorithms]
tags: [algorithm, sort]
---
{% include JB/setup %}

first

	<?php
	/* 挖坑法
	 * 条件：
	 * 1. 坑必须位于边界
	 * 2. 基准值需是坑所在的值
	 * 上面的两个条件得出基准值必须是边界值，如果条件指定基准值需是中间值或其他位置的值，
	 * 可以将其与边界值进行替换，使其变成边界值。
	 */
	class QuickSort {
		// 以边界值为基准数
		public static function qs1(&$arr, $low, $high) {
			if (empty($arr)) {
				return;
			}
			if ($low >= $high) {
				return;
			}

			$oriLow = $low;
			$oriHigh = $high;
			$x = $arr[$low];

			self::partition($arr, $low, $high, $x);
			self::qs1($arr, $oriLow, $low - 1);
			self::qs1($arr, $low + 1, $oriHigh);

			return true;
		}

		// 以中间值为基准数
		public static function qs2(&$arr, $low, $high) {
			if (empty($arr)) {
				return;
			}
			if ($low >= $high) {
				return;
			}

			$oriLow = $low;
			$oriHigh = $high;
			$middle = (int) (($low + $high) / 2);
			$x = $arr[$middle];

			// 交换位置，使要作为基准值的中间值变成边界值
			$tmp = $arr[$low];
			$arr[$low] = $arr[$middle];
			$arr[$middle] = $tmp;

			self::partition($arr, $low, $high, $x);
			self::qs2($arr, $oriLow, $low - 1);
			self::qs2($arr, $low + 1, $oriHigh);
			
			return true;
		}

		// 根据基准值的值，确定基准值的位置，然后确定了左右边界
		private static function partition(&$arr, &$low, &$high, $x) {
			while($low < $high) {
				while($low < $high && $arr[$high] >= $x) {
					$high--;
				}
				if ($low < $high) {
					$arr[$low] = $arr[$high];
					$low++;
				}
				while($low < $high && $arr[$low] < $x) {
					$low++;
				}
				if ($low < $high) {
					$arr[$high] = $arr[$low];
					$high--;
				}
			}
			$arr[$low] = $x;

			return true;
		}
	}

	$arr = range(1, 10);
	shuffle($arr);
	array_splice($arr, rand(0, 7), 2, array(rand(1, 10), rand(1, 10)));

	echo "before\n";
	print_r($arr);
	echo "\n";
	echo "after\n";
	QuickSort::qs2($arr, 0, count($arr)-1);
	print_r($arr);
	
second

	<?php
	/* 包围法
	 * 天生可指定任意位置的基准值
	 */
	class QuickSort {
		public static function qs1(&$arr, $low, $high) {
			if (empty($arr)) {
				return;
			}
			if ($low >= $high) {
				return;
			}

			$oriLow = $low;
			$oriHigh = $high;
			$middle = (int) (($low + $high) / 2);
			$x = $arr[$middle];

			self::partition($arr, $low, $high, $x);
			if ($oriLow < $high) {
				self::qs1($arr, $oriLow, $high);
			}
			if ($oriHigh > $low) {
				self::qs1($arr, $low, $oriHigh);
			}
			return true;
		}

		// 根据基准值的值，确定了左右边界
		private function partition(&$arr, &$low, &$high, $x) {
			// 和挖坑法的low和high相等则代表到了可以放置基准值的位置不同，
			// 包围法的low和high相等代表到了将要决定出左右边界的前夕
			while ($low <= $high) {
				// 和挖坑法不同，此处不能有等号
				// 例如：数组为[1, 3, 3, 2]，基准值为3，如果加等号，这种情况会造成死循环
				while ($arr[$low] < $x) {
					$low++;
				}
				while ($arr[$high] > $x) {
					$high--;
				}

				// low和high相等时
				// 如果他们对应的值和基准值也相等
				// 那下面的交换操作确实是一次浪费，但是++和--还是有用的
				// 如果和基准值不相等，下面的操作不会被执行
				if ($low <= $high) {
					$tmp = $arr[$low];
					$arr[$low] = $arr[$high];
					$arr[$high] = $tmp;
					$low++;
					$high--;
				}
			}
			return true;	
		}
	}

	$arr = range(1, 10);
	shuffle($arr);
	array_splice($arr, rand(0, 7), 2, array(rand(1, 10), rand(1, 10)));

	echo "before\n";
	print_r($arr);
	echo "\n";
	echo "after\n";
	QuickSort::qs1($arr, 0, count($arr)-1);
	print_r($arr);