package com.test.home.directMessage

import android.util.Log
import androidx.recyclerview.widget.LinearLayoutManager
import androidx.recyclerview.widget.RecyclerView

abstract class ChatRecyclerOnScrollListener : RecyclerView.OnScrollListener() {

  /**
   * The total number of items in the dataset after the last load
   */
  private var mPreviousTotal = 0
  /**
   * True if we are still waiting for the last set of data to load.
   */
  private var mLoading = true

  private var loadedAll = false

  override fun onScrolled(recyclerView: RecyclerView, dx: Int, dy: Int) {
    super.onScrolled(recyclerView, dx, dy)

    val visibleItemCount = recyclerView.childCount
    val totalItemCount = recyclerView.layoutManager!!.itemCount
    val firstVisibleItem =
      (recyclerView.layoutManager as LinearLayoutManager).findFirstVisibleItemPosition()

    if (mLoading) {
      if (totalItemCount > mPreviousTotal) {
        mLoading = false
        mPreviousTotal = totalItemCount
      }
    }
    val visibleThreshold = 5
    // if (!mLoading && !loadedAll && totalItemCount - visibleItemCount <= firstVisibleItem + visibleThreshold) {
    if (!mLoading && !loadedAll && firstVisibleItem == 0) {
      // End has been reached

      onLoadMore(true)

      mLoading = true
    }
  }

  fun loadedAllChats(loaded: Boolean) {
    loadedAll = loaded
  }

  abstract fun onLoadMore(hasData: Boolean)
}
