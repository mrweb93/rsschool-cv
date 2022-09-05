# Evgenii Sobolev
## Contacts
* Phone: +79141622610
* Email: mr_w_e_b@mail.ru
* telegram: @mr_w_e_b
* discord: mr_w_e_b(@mrweb93)
## About
My goal is grow up every day. I am lerning Kotlin for develop on Android. I work in IT company. We create application for citezens city.
## Skills
* Java
* Kotlin
* Hard communication

## Example code
```
package io.github.mrweb93.fiveletters

import android.content.Intent
import android.net.Uri
import android.os.Bundle
import android.util.Log
import android.view.View
import android.widget.TextView
import android.widget.Toast
import androidx.appcompat.app.AppCompatActivity
import androidx.lifecycle.ViewModelProviders
import java.util.*


private const val TAG = "GameActivity"

private const val KEY_ANSWER = "answer"
private const val KEY_ANSWER_USER = "answer_user"
private const val KEY_ROW_NUMBER = "row_number"
private const val KEY_LETTER_NUMBER = "letter_number"
private const val KEY_BOARD_CELL_TEXT = "board_cell_text"
private const val KEY_BOARD_CELL_BACKGROUND = "board_cell_background"
private const val KEY_BOARD_CELL_COLOR_TEXT = "board_cell_color_text"
private const val KEY_KEYBOARD_BACKGROUND = "keyboard_background"
private const val KEY_KEYBOARD_COLOR_TEXT = "keyboard_color_text"

class MainActivity : AppCompatActivity() {

    private lateinit var board: List<TextView>
    private lateinit var keyboard:List<TextView>
    private lateinit var score:List<TextView>


    private val gameViewModel: GameViewModel by lazy {
        ViewModelProviders.of(this).get(GameViewModel::class.java)
    }

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_main)

        Log.d(TAG, UUID.randomUUID().toString())

        gameViewModel.updateAnswer()

        gameViewModel.game.answer =
            savedInstanceState?.getString(KEY_ANSWER, gameViewModel.game.answer)
                ?: gameViewModel.game.answer
        gameViewModel.game.row_number = savedInstanceState?.getInt(KEY_ROW_NUMBER, 0) ?: 0
        gameViewModel.game.letter_number = savedInstanceState?.getInt(KEY_LETTER_NUMBER, 0) ?: 0
        gameViewModel.game.answer_user = savedInstanceState?.getString(KEY_ANSWER_USER, "") ?: ""
        gameViewModel.board_cell_text = savedInstanceState?.getStringArrayList(KEY_BOARD_CELL_TEXT) ?: gameViewModel.board_cell_text
        gameViewModel.board_cell_background = savedInstanceState?.getIntegerArrayList(KEY_BOARD_CELL_BACKGROUND) ?: gameViewModel.board_cell_background
        gameViewModel.board_cell_color_text = savedInstanceState?.getIntegerArrayList(KEY_BOARD_CELL_COLOR_TEXT) ?: gameViewModel.board_cell_color_text
        gameViewModel.keyboard_background = savedInstanceState?.getIntegerArrayList(KEY_KEYBOARD_BACKGROUND) ?: gameViewModel.keyboard_background
        gameViewModel.keyboard_color_text = savedInstanceState?.getIntegerArrayList(KEY_KEYBOARD_COLOR_TEXT) ?: gameViewModel.keyboard_color_text


        board = listOf(
            findViewById(R.id.row11_textview),findViewById(R.id.row12_textview), findViewById(R.id.row13_textview), findViewById(R.id.row14_textview), findViewById(R.id.row15_textview),
            findViewById(R.id.row21_textview),findViewById(R.id.row22_textview), findViewById(R.id.row23_textview), findViewById(R.id.row24_textview), findViewById(R.id.row25_textview),
            findViewById(R.id.row31_textview),findViewById(R.id.row32_textview), findViewById(R.id.row33_textview), findViewById(R.id.row34_textview), findViewById(R.id.row35_textview),
            findViewById(R.id.row41_textview),findViewById(R.id.row42_textview), findViewById(R.id.row43_textview), findViewById(R.id.row44_textview), findViewById(R.id.row45_textview),
            findViewById(R.id.row51_textview),findViewById(R.id.row52_textview), findViewById(R.id.row53_textview), findViewById(R.id.row54_textview), findViewById(R.id.row55_textview),
            findViewById(R.id.row61_textview),findViewById(R.id.row62_textview), findViewById(R.id.row63_textview), findViewById(R.id.row64_textview), findViewById(R.id.row65_textview)
        )

        keyboard = listOf(
            findViewById(R.id.q_textview),findViewById(R.id.w_textview), findViewById(R.id.e_textview), findViewById(R.id.r_textview), findViewById(R.id.t_textview), findViewById(R.id.y_textview),findViewById(R.id.u_textview), findViewById(R.id.i_textview), findViewById(R.id.o_textview), findViewById(R.id.p_textview),
            findViewById(R.id.a_textview),findViewById(R.id.s_textview), findViewById(R.id.d_textview), findViewById(R.id.f_textview), findViewById(R.id.g_textview), findViewById(R.id.h_textview),findViewById(R.id.j_textview), findViewById(R.id.k_textview), findViewById(R.id.l_textview),
            findViewById(R.id.z_textview), findViewById(R.id.x_textview),findViewById(R.id.c_textview), findViewById(R.id.v_textview), findViewById(R.id.b_textview), findViewById(R.id.n_textview), findViewById(R.id.m_textview)
        )

        score = listOf(
            findViewById(R.id.logo1_textview), findViewById(R.id.logo2_textview), findViewById(R.id.logo3_textview), findViewById(R.id.logo4_textview),
            findViewById(R.id.logo5_textview), findViewById(R.id.logo6_textview), findViewById(R.id.logo7_textview), findViewById(R.id.logo8_textview)
        )



        gameViewModel.gameStart(board,keyboard)

    }

    override fun onSaveInstanceState(savedInstanceState: Bundle) {
        super.onSaveInstanceState(savedInstanceState)

        savedInstanceState.putString(KEY_ANSWER, gameViewModel.game.answer)
        savedInstanceState.putString(KEY_ANSWER_USER, gameViewModel.game.answer_user)
        savedInstanceState.putInt(KEY_ROW_NUMBER, gameViewModel.game.row_number)
        savedInstanceState.putInt(KEY_LETTER_NUMBER, gameViewModel.game.letter_number)
        savedInstanceState.putStringArrayList(KEY_BOARD_CELL_TEXT, gameViewModel.board_cell_text)
        savedInstanceState.putIntegerArrayList(KEY_BOARD_CELL_BACKGROUND, gameViewModel.board_cell_background)
        savedInstanceState.putIntegerArrayList(KEY_BOARD_CELL_COLOR_TEXT, gameViewModel.board_cell_color_text)
        savedInstanceState.putIntegerArrayList(KEY_KEYBOARD_BACKGROUND, gameViewModel.keyboard_background)
        savedInstanceState.putIntegerArrayList(KEY_KEYBOARD_COLOR_TEXT, gameViewModel.keyboard_color_text)

    }

    fun checkAnswer(view: View) {
        if (gameViewModel.game.letter_number != 5 && gameViewModel.game.row_number != 6) {
            Toast.makeText(this, "Your word does not have 5 letters", Toast.LENGTH_SHORT).show()
        } else {
            if (gameViewModel.game.row_number != 6) {

                when (gameViewModel.game.row_number) {
                    0 -> gameViewModel.setColorCell(listOf(board[0], board[1], board[2], board[3], board[4]),keyboard)
                    1 -> gameViewModel.setColorCell(listOf(board[5], board[6], board[7], board[8], board[9]),keyboard)
                    2 -> gameViewModel.setColorCell(listOf(board[10], board[11], board[12], board[13], board[14]),keyboard)
                    3 -> gameViewModel.setColorCell(listOf(board[15], board[16], board[17], board[18], board[19]),keyboard)
                    4 -> gameViewModel.setColorCell(listOf(board[20], board[21], board[22], board[23], board[24]),keyboard)
                    5 -> gameViewModel.setColorCell(listOf(board[25], board[26], board[27], board[28], board[29]),keyboard)
                }

                if (gameViewModel.game.answer_user == gameViewModel.game.answer) {
                    GameOverDialog("Win Game","Your answer is correct. Answer is " + gameViewModel.game.answer,true).show(supportFragmentManager, TAG)
                  gameViewModel.cleanGame(board, keyboard)
                  gameViewModel.game.score++
                  gameViewModel.setScore(score)

                } else {
                    gameViewModel.rowNumberNext()
                    gameViewModel.game.letter_number = 0
                }

            } else {
                GameOverDialog("Game Over","Answer is " + gameViewModel.game.answer, false).show(supportFragmentManager, TAG)
               gameViewModel.cleanGame(board, keyboard)
                if (gameViewModel.game.score!=0) gameViewModel.game.score--
                gameViewModel.setScore(score)
            }
        }
    }

    fun openDonatePage(view: View) {
        val browserIntent = Intent(Intent.ACTION_VIEW, Uri.parse("https://sobe.ru/na/s2T2S0Z6B2q6"))
        startActivity(browserIntent)
    }


    fun deleteLetter(view: View) {
        if (gameViewModel.game.letter_number != 0) gameViewModel.letterNumberPrev()
        gameViewModel.setLetter(
            board,
            gameViewModel.game.row_number,
            gameViewModel.game.letter_number,
            ""
        )
    }

    fun addLetter(view: View) {
        val letter: String = findViewById<TextView>(view.id).text.toString()

        if (gameViewModel.game.letter_number != 5) {
            gameViewModel.setLetter(
                board,
                gameViewModel.game.row_number,
                gameViewModel.game.letter_number,
                letter
            )
            gameViewModel.letterNumberNext()
        }

    }

    fun onShowDialog(view: View) {
        InstructionDialog().show(supportFragmentManager, TAG)
    }
}
```
## My apps
* https://play.google.com/store/apps/details?id=io.github.mrweb93.fiveletters
* https://play.google.com/store/apps/details?id=ru.mmrescue.mobileapp