---
layout:     post
title:      JavaExercise4Tmp
subtitle:   java timer类&thread
date:       2017-10-26
author:     CoderFrank	
header-img: img/post-bg-unix-linux.jpg
catalog: true
tags:
    - Java
---

```java
package com.TimerTest;

import javax.swing.*;
import java.awt.*;
import java.awt.event.ActionEvent;
import java.awt.event.ActionListener;
import java.util.Timer;
import java.util.TimerTask;

public class Exercise4 extends JFrame{
    private Timer timer;
    private JPanel jPanel;
    private JButton jButtonStart;
    private JButton jButtonStop;
    private JLabel jLabelText;
    private int i = 0;
    public Exercise4(){
        jPanel = new JPanel();
        jButtonStart = new JButton("Start");
        jButtonStop = new JButton("Stop");
        jLabelText = new JLabel("0");


    }

    public void prepareUI(){

        jButtonStart.addActionListener(new ActionListener() {
            @Override
            public void actionPerformed(ActionEvent e) {
                timer = new Timer();
                timer.schedule(new TimerTask() {
                    @Override
                    public void run() {
                        ++i;
                        jLabelText.setText(""+i);
                    }
                },0,1000);
            }
        });

        jButtonStop.addActionListener(new ActionListener() {
            @Override
            public void actionPerformed(ActionEvent e) {
                timer.cancel();
            }
        });

        jPanel.add(jButtonStart);
        jPanel.add(jButtonStop);
        jPanel.add(jLabelText);

        this.add(jPanel);
        this.setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
        this.setSize(new Dimension(300,100));
//        this.pack();
        this.setVisible(true);
    }




    public static void main(String args[]) {
        Exercise4 exercise5 = new Exercise4();
        exercise5.prepareUI();
    }
}

```

