---
title: Leetcode_355
tags:
  - Leetcode
categories:
  - Technology
  - Leetcode
date: 2016-09-02 16:36:00
---
Design Twitter

<!-- more -->

***

### Problem
Design a simplified version of Twitter where users can post tweets, follow/unfollow another user and is able to see the 10 most recent tweets in the user's news feed. Your design should support the following methods:

postTweet(userId, tweetId): Compose a new tweet.
getNewsFeed(userId): Retrieve the 10 most recent tweet ids in the user's news feed. Each item in the news feed must be posted by users who the user followed or by the user herself. Tweets must be ordered from most recent to least recent.
follow(followerId, followeeId): Follower follows a followee.
unfollow(followerId, followeeId): Follower unfollows a followee.

### Solution 
这样写空间占用小
但是时间复杂度高，其实不好。
{% codeblock lang:java  %}
public class Twitter {
	Map<Integer, Set<Integer>> follow;
	List<Post> postList;

	/** Initialize your data structure here. */
	public Twitter() {
		follow = new HashMap<Integer, Set<Integer>>();
		postList = new ArrayList<Post>();
	}
	
	/** Compose a new tweet. */
	public void postTweet(int userId, int tweetId) {
		postList.add(new Post(tweetId, userId));
		if(!follow.containsKey(userId)) {
			Set<Integer> set = new HashSet<Integer>();
			set.add(userId);
			follow.put(userId, set);
		}
	}
	
	/** Retrieve the 10 most recent tweet ids in the user's news feed. Each item in the news feed must be posted by users who the user followed or by the user herself. Tweets must be ordered from most recent to least recent. */
	public List<Integer> getNewsFeed(int userId) {
		Set<Integer> set = follow.get(userId);
		
		List<Integer> result = new ArrayList<Integer>();
		
		if(set != null) {
			int i = postList.size() - 1; 
		
			while(i >= 0 && result.size()< 10) {
				Post post = postList.get(i);
				int uId = post.getUserId();
				if(set.contains(uId)) {
					result.add(post.getPostId());
				}
				i--;
			}
		}
		
		
		return result;
		
	}
	
	/** Follower follows a followee. If the operation is invalid, it should be a no-op. */
	public void follow(int followerId, int followeeId) {
		if(!follow.containsKey(followerId)) {
			Set<Integer> set = new HashSet<Integer>();
			set.add(followerId);
			set.add(followeeId);
			follow.put(followerId, set);
		} else {
			follow.get(followerId).add(followeeId);
		}
	}
	
	/** Follower unfollows a followee. If the operation is invalid, it should be a no-op. */
	public void unfollow(int followerId, int followeeId) {
		if ( follow.get(followerId) != null && followerId != followeeId) {
			follow.get(followerId).remove(followeeId);
		}
		
	}
	
	private class Post {
		int postId; 
		int userId;
		
		public Post(int postId, int userId) {
			this.postId = postId;
			this.userId = userId;
		}
		
		public int getPostId() {
			return postId;
		}
		
		public int getUserId() {
			return userId;
		}
	}
{% endcodeblock %}




*** 

Over！










































